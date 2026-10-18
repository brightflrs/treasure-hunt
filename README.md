# 🏴‍☠️ PacketCapture CTF — Treasure Hunt Trip

> **Challenge Category:** OSINT / Geolocation  
> **Difficulty:** Easy–Medium  
> **Flag:** `PacketCapture{85.36_81.05}`

---

## 📋 Mission Brief

> My grandfather left me this image as part of his will. He said I can find a treasure using it. I don't know, maybe he's just playing with me. I don't know if **3 words** would even suffice.
>
> **File:** `treasurehunttrip.jpg`  
> **Coordinate Type:** DMS — convert from `LeadNo°TrailNo`  
> **Flag Format:** `PacketCapture{00.00_00.00}`

---

## 🚩 Final Flag

```text
PacketCapture{85.36_81.05}
```

---

## 🗺️ Solution / Walkthrough

### Step 1 — Download and Inspect the Image

Download `treasurehunttrip.jpg` from the provided link and inspect it.

Checking the metadata / JPEG comment reveals a decoy:

```bash
exiftool treasurehunttrip.jpg
```

```text
Comment : If you're looking here, then you've searched yourself into a wrong hole.
```

This confirms that the answer is **not** hidden in the EXIF/metadata — it's a rabbit hole. 🐇

Other common steganography checks (LSB, `steghide`, `binwalk`, appended files) also do not yield the flag.

---

### Step 2 — Notice the "3 Words" Hint

The key hint in the brief is:

> *"I don't know if **3 words** would even suffice."*

This is a reference to **[what3words](https://what3words.com/)** — a geolocation system that assigns a unique combination of **3 words** to every 3m × 3m square on Earth.

Now look at the filename:

```text
treasurehunttrip.jpg
```

Split it into three words:

```text
treasure . hunt . trip
```

That gives us the what3words address:

```text
///treasure.hunt.trip
```

🔗 https://what3words.com/treasure.hunt.trip

---

### Step 3 — Get the Coordinates

Resolving `///treasure.hunt.trip` on what3words gives the following coordinates (Decimal Degrees):

```text
Latitude:  -85.600191
Longitude: -81.091278
```

Location: Antarctica region (South / West coordinates).

---

### Step 4 — Convert to DMS

Convert the Decimal Degrees coordinates to **DMS (Degrees, Minutes, Seconds)**:

**Latitude**
```text
85.600191°
= 85° 36' 00.69" S
```

**Longitude**
```text
81.091278°
= 81° 05' 28.60" W
```

---

### Step 5 — Apply the `LeadNo°TrailNo` Conversion

Per the mission brief:

> **Coordinate Type: DMS convert from LeadNo°TrailNo**

Take the **leading number** (degrees, before the `°` symbol) and the **trailing number** (minutes, after the `°` symbol), then join them with a dot:

```text
85°36'  →  85.36
81°05'  →  81.05
```

Combine latitude and longitude with an underscore, following the flag format `PacketCapture{00.00_00.00}`:

```text
PacketCapture{85.36_81.05}
```

✅ **Flag accepted!**

---

## ❌ Common Wrong Attempt

A tempting but incorrect approach is to simply round the Decimal Degrees coordinates to 2 decimal places:

```text
PacketCapture{-85.60_-81.09}   ❌ Wrong
```

This fails because the brief specifically requires the **DMS → LeadNo.TrailNo** conversion, not Decimal Degrees rounding.

| Method | Latitude | Longitude | Result |
|--------|----------|-----------|--------|
| Decimal Degrees (rounded) ❌ | -85.60 | -81.09 | `PacketCapture{-85.60_-81.09}` |
| **DMS Lead°Trail → Lead.Trail** ✅ | **85.36** | **81.05** | **`PacketCapture{85.36_81.05}`** |

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| [what3words](https://what3words.com/treasure.hunt.trip) | Convert the 3-word address to coordinates |
| [ExifTool](https://exiftool.org/) | Inspect image metadata (found the decoy comment) |
| Google Maps | Verify coordinates / DMS conversion |
| [OSINT Framework](https://mail.osintframework.com/) | Geolocation tool discovery |

---

## 🔑 Key Takeaways

1. **Read the brief carefully** — phrases like *"3 words would even suffice"* are deliberate hints (what3words).
2. **Filenames are clues too** — `treasurehunttrip` was literally the what3words address.
3. **Beware of decoys** — the JPEG comment explicitly tells you metadata is the "wrong hole."
4. **Follow the exact flag format/conversion instruction** — `LeadNo°TrailNo` means degrees°minutes → `degrees.minutes`, not rounded decimal degrees.

---

## 📁 Files

| File | Description |
|------|-------------|
| `treasurehunttrip.jpg` | Original challenge image provided via MediaFire |

---

*Writeup for CTF challenge "Treasure Hunt Trip" — PacketCapture CTF.*
