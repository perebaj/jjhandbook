# Prometheus remote write

Protocol designed to make it easy propagate samples in real-time from a sender to a receiver.

The Remote-Write protocol is designed to be stateless; it means that we don't save the previous state of the sender to be used in the next requests.

# Glossary

For the purposes of this document the following definitions MUST be followed:

- a "Sender" is something that sends Prometheus Remote Write data.
- a "Receiver" is something that receives Prometheus Remote Write data.
- a "Sample" is a pair of (timestamp, value).
- a "Label" is a pair of (key, value).
- a "Series" is a list of samples, identified by a unique set of labels.

# Important Resources

- https://prometheus.io/docs/specs/remote_write_spec/#:~:text=The%20remote%20write%20protocol%20is,is%20not%20considered%20%22streaming%22.
