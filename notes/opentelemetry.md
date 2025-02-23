# OpenTelemetry collector

Vendor agnostic way to receive process and export telemetry data

```mermaid
graph TD

```

# Resources

- [OpenTelemetry Collector Deployment Patterns - Juraci Paixão Kröhling, Red Hat](https://www.youtube.com/watch?v=WhRrwSHDBFs&ab_channel=CNCF%5BCloudNativeComputingFoundation%5D)


# Prometheus -> Opentelemetry transformation

To create a mental map for transforming Prometheus metrics into the OpenTelemetry Protocol (OTLP), we need to outline the key concepts and steps involved in converting data from the Prometheus ecosystem (including its exposition format or OpenMetrics) into OTLP’s metric data model. The process involves understanding the structure of Prometheus metrics, how they are represented, and how they align with OTLP’s structure, based on the compatibility specification you referenced. Below is a mental map to guide this transformation, presented in a clear, step-by-step conceptual framework.

---

### Mental Map: Transforming Prometheus Metrics to OTLP

#### 1. Understand Prometheus Metric Structure
   - **Source Format**: Prometheus uses a text-based exposition format (or OpenMetrics, its superset) with a pull-based model.
   - **Core Components**:
     - **Metric Name**: A unique identifier (e.g., `http_requests_total`).
     - **Type**: One of `Counter`, `Gauge`, `Histogram`, or `Summary` (plus OpenMetrics additions like `Info` or `StateSet`).
     - **Labels**: Key-value pairs for dimensionality (e.g., `method="GET", status="200"`).
     - **Value**: A numeric value (e.g., `123.45`).
     - **Timestamp**: Optional, in milliseconds (usually not present in pull model).
     - **Metadata**: Includes `HELP` (description) and `UNIT` (unit of measure, optional in OpenMetrics).
     - **Exemplars**: Optional annotations linking to traces (OpenMetrics-specific).
   - **Example**:
     ```
     # TYPE http_requests_total counter
     # HELP http_requests_total Total HTTP requests
     http_requests_total{method="GET", status="200"} 123.45
     ```

#### 2. Identify OTLP Metric Structure
   - **Target Format**: OTLP uses a protocol buffer-based model for metrics, logs, and traces, designed for push or pull workflows.
   - **Core Components**:
     - **Metric**: A container with a name, description, unit, and data points.
     - **Data Points**: Individual measurements with:
       - **Value**: Can be int64, double, or aggregated (e.g., histogram).
       - **Timestamp**: Required, in nanoseconds.
       - **Attributes**: Key-value pairs (similar to Prometheus labels).
       - **Start Timestamp**: For cumulative metrics to track resets.
     - **Type**: Supports `Gauge`, `Sum` (monotonic or non-monotonic), `Histogram`, `ExponentialHistogram`, or `Summary`.
     - **Instrumentation Scope**: Metadata about the source (e.g., library name and version).
     - **Resource**: Contextual info about the entity producing the metric (e.g., service name).
   - **Aggregation Temporality**: Options are `Cumulative` (default in Prometheus) or `Delta`.

#### 3. Map Prometheus to OTLP
   - **Metric Name**:
     - Prometheus: `http_requests_total`.
     - OTLP: Use as the `name` field directly (e.g., `http_requests_total`).
     - Note: Optionally strip suffixes like `_total` or `_seconds` via configuration for cleaner naming.
   - **Type Conversion**:
     - Prometheus `Counter` → OTLP `Sum` (monotonic, cumulative).
     - Prometheus `Gauge` → OTLP `Gauge`.
     - Prometheus `Histogram` → OTLP `Histogram` (cumulative).
     - Prometheus `Summary` → OTLP `Summary`.
     - OpenMetrics `Info` → OTLP `Gauge` (non-monotonic sum with metadata).
     - OpenMetrics `StateSet` → OTLP `Gauge` (non-monotonic sum with specific attributes).
   - **Labels to Attributes**:
     - Prometheus labels (e.g., `method="GET"`) → OTLP `attributes` (same key-value pairs).
     - Special handling:
       - Drop `otel_scope_name` and `otel_scope_version` labels and move to OTLP `InstrumentationScope`.
       - Drop `otel_scope_info` metrics and use their labels as scope attributes.
   - **Value**:
     - Prometheus numeric value → OTLP data point `value` (match type: int64 or double).
   - **Timestamp**:
     - Prometheus: Often absent (implied by scrape time).
     - OTLP: Set to scrape time in nanoseconds; if present in Prometheus, convert milliseconds to nanoseconds.
   - **Metadata**:
     - `HELP` → OTLP `description`.
     - `UNIT` → OTLP `unit` (translate units, e.g., `seconds` to `s`).
   - **Exemplars**:
     - Prometheus/OpenMetrics exemplars → OTLP exemplars:
       - Map `trace_id` and `span_id` labels to OTLP trace and span IDs.
       - Other labels → OTLP exemplar attributes.
       - Timestamp (if present) → OTLP exemplar timestamp.

#### 4. Handle Contextual Elements
   - **Instrumentation Scope**:
     - If `otel_scope_name` and `otel_scope_version` labels exist, use them to populate OTLP `InstrumentationScope`.
     - Otherwise, assign a default scope (e.g., from the translating entity like an OpenTelemetry Collector).
   - **Resource**:
     - Prometheus doesn’t natively separate resource info; infer from labels or external config (e.g., `service.name`).
     - Add as OTLP `Resource` attributes (e.g., `service.name="my-app"`).
   - **Temporality**:
     - Prometheus uses cumulative by default → OTLP `Cumulative` temporality.
     - Note: OTLP supports `Delta`, but Prometheus doesn’t; conversion may require state tracking.

#### 5. Step-by-Step Transformation Process
   1. **Parse Prometheus Data**:
      - Read metric family (name, type, labels, value, metadata).
   2. **Create OTLP Metric**:
      - Set `name` from Prometheus metric name.
      - Set `description` from `HELP`.
      - Set `unit` from `UNIT` (if present).
   3. **Convert Type and Data Points**:
      - Map type (e.g., `Counter` to `Sum`).
      - Create data points with values, attributes (labels), and timestamp (scrape time).
   4. **Handle Special Cases**:
      - Move scope-related labels to `InstrumentationScope`.
      - Drop unsupported features (e.g., native histograms if not supported).
   5. **Add Resource Context**:
      - Populate `Resource` with inferred or configured attributes.
   6. **Serialize to OTLP**:
      - Encode into OTLP protobuf format for transmission.

#### 6. Example Transformation
   - **Prometheus Input**:
     ```
     # TYPE http_requests_total counter
     # HELP http_requests_total Total HTTP requests
     # UNIT requests
     http_requests_total{method="GET", status="200"} 123.45
     ```
   - **OTLP Output (Conceptual)**:
     - Metric:
       - `name`: `http_requests_total`
       - `description`: `Total HTTP requests`
       - `unit`: `requests`
       - `data`: `Sum`
         - `is_monotonic`: true
         - `aggregation_temporality`: `Cumulative`
         - Data Point:
           - `value`: `123.45` (double)
           - `attributes`: `{method: "GET", status: "200"}`
           - `timestamp`: `<scrape_time_in_ns>`
           - `start_timestamp`: `<initial_time_in_ns>`
       - `InstrumentationScope`: (default or derived)
       - `Resource`: (e.g., `service.name="my-app"`)

#### 7. Key Considerations
   - **Lossless Conversion**: Ensure all Prometheus data (labels, exemplars) is preserved unless explicitly unsupported (e.g., native histograms).
   - **Configuration Options**: Allow toggling suffix removal or scope label handling.
   - **Statefulness**: For counters, track `start_timestamp` to handle resets (needs stateful processor if converting live).

---

### Summary
This mental map bridges Prometheus’s simpler, metric-focused model to OTLP’s richer, multi-signal framework. Start with the metric name and type, map labels to attributes, handle metadata, and layer on OTLP-specific context like scope and resource. The process is mostly straightforward but requires attention to temporality and special features like exemplars. With this structure, you can mentally walk through any Prometheus metric and envision its OTLP equivalent!
