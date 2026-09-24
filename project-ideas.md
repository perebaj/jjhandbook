# Project ideas

I'm always thinking in something new to implement. But sometimes I forget them. Documenting...

# Esaj Collector

Centralize all information about law courts in Brazil. It's a big project. Starting simple with a crawler to collect data from the Esaj website.
Enrich the data, capturing the hide information from the PDFs.
Serve the data in a REST API.

# Jammer

Play some notes in your Guitar and jammer will complement your music using AI.

References:
- https://github.com/schollz/PIanoAI
- https://infinitedigits.co/tinker/piano-ai/


# Trigger a spotify song using a voice command

# Sentiment analysis using the leaked list of influencers ✅

# Museum guide

# Take care of my plants using embedded systems

# RFID detector for home lighters

Tag every lighter at home with an RFID chip and put a reader at the door. If a tagged lighter crosses the doorway, an alarm goes off — so nobody walks out with one.

Key constraint: cheap 13.56MHz readers (RC522) only read at ~3cm — useless at a doorway. It has to be UHF RFID (860–960MHz, EPC Gen2), the same tech as retail anti-theft gates: 1–5m range with passive (battery-free) tags small enough for a lighter.

Shopping list (~US$60–120 total):

- UHF reader module (M100/R200 based, e.g. YRM100) — talks UART
- Circular-polarized UHF antenna (5–9 dBi) + short SMA/IPEX coax — covers the door frame, reads tags in any orientation
- Passive UHF sticker tags for plastic lighters (Bic); on-metal tags for metal ones (Zippo)
- ESP32 — receives the reads, decides when to alarm; WiFi for phone notifications
- Active buzzer or 12V siren + MOSFET/relay
- 5V power supply (or 12V + regulator if using a siren), enclosure, mounting
- Reed switch on the door — only arm detection when the door opens, otherwise the reader keeps alarming on lighters sitting on the couch
- Optional: second antenna on the other side of the frame — the body attenuates UHF, and a lighter inside a pocket is the worst-case read
