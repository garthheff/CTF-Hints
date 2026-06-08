# Bugged

John likes to live in a very Internet connected world. Maybe too connected...

John was working on his smart home appliances when he noticed weird traffic going across the network. Can you help him figure out what these weird network communications are?

Room: https://tryhackme.com/room/bugged

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/bugged.md

--------

<details>
<summary>Hint 1</summary>

Start with full port enumeration. There are not many open ports, so focus on the unusual service rather than SSH.

</details>

<details>
<summary>Hint 2</summary>

Port `1883` is MQTT. Use MQTT client tools to subscribe to broker topics.

</details>

<details>
<summary>Hint 3</summary>

Subscribe to all MQTT topics and watch the live output.

```bash
mosquitto_sub -h TARGET_IP -p 1883 -t '#' -v
```

</details>

<details>
<summary>Hint 4</summary>

Most of the messages look like smart-home telemetry, but not all of them are just sensor data. Look carefully for a topic that looks like configuration.

</details>

<details>
<summary>Hint 5</summary>

One topic ends with `/config`. The payload is encoded.

</details>

<details>
<summary>Hint 6</summary>

Try decoding suspicious payloads with base64.

```bash
echo 'PASTE_PAYLOAD_HERE' | base64 -d
```

</details>

<details>
<summary>Hint 7</summary>

The decoded config gives you three useful things:

* a backdoor ID
* a publish topic
* a subscribe topic

</details>

<details>
<summary>Hint 8</summary>

The backdoor supports a small set of registered commands. One of them can execute system commands.

</details>

<details>
<summary>Hint 9</summary>

If you send the wrong message format, the service tells you what format it expects. Decode the response if it looks unreadable.

</details>

<details>
<summary>Hint 10</summary>

The expected message format is a base64 encoded JSON object.

It follows this structure:

```json
{"id":"BACKDOOR_ID","cmd":"COMMAND","arg":"ARGUMENT"}
```

</details>

<details>
<summary>Hint 11</summary>

Publish the encoded JSON to the hidden subscribe topic, then listen on the hidden publish topic for the response.

</details>

<details>
<summary>Hint 12</summary>

Create a listener that automatically decodes responses.

```bash
mosquitto_sub -h TARGET_IP -p 1883 \
  -t 'PUB_TOPIC_HERE' \
  -v | while read topic payload; do echo "[$topic]"; echo "$payload" | base64 -d; echo; done
```

</details>

<details>
<summary>Hint 13</summary>

Test command execution with a harmless command first.

```bash
id
```

</details>

<details>
<summary>Hint 14</summary>

A reverse shell may not be required. If command output is returned over MQTT, you can keep using MQTT as your command channel.

</details>

<details>
<summary>Hint 15</summary>

Search for likely flag files.

```bash
find / -name flag.txt 2>/dev/null
```

</details>

<details>
<summary>Hint 16</summary>

The flag is in the challenge user's home directory.

</details>

## Final Spoiler

<details>
<summary>Final Spoiler</summary>

The MQTT broker exposes a base64 encoded config topic. Decoding it reveals a hidden MQTT backdoor with a backdoor ID, publish topic, subscribe topic, and registered commands.

Messages must be sent as base64 encoded JSON in this format:

```json
{"id":"BACKDOOR_ID","cmd":"CMD","arg":"COMMAND_TO_RUN"}
```

Use the `CMD` command to run shell commands through MQTT.

The flag path is:

```text
/home/challenge/flag.txt
```

Read it with:

```bash
cat /home/challenge/flag.txt
```

Flag format:

```text
flag{REDACTED}
```

</details>
