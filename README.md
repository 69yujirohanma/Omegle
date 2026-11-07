<div align="center">

<img src="logo.svg" alt="Randomly logo" width="96" height="96">

# Randomly: Free Omegle Alternative – Random Video Chat & Text Chat with Strangers

**An open-source Omegle clone and Omegle alternative that runs entirely on GitHub Pages.**
Talk to strangers with random video chat or anonymous text chat. No sign-up, shared interests, mobile friendly, with built-in IP privacy protection.

[**Live site: 69yujirohanma.github.io/Omegle**]([https://69yujirohanma.github.io/Omegle/](https://69yujirohanma.github.io/Omegle/))

![WebRTC](https://img.shields.io/badge/WebRTC-peer--to--peer-f43f5e) ![GitHub Pages](https://img.shields.io/badge/hosted%20on-GitHub%20Pages-222) ![No sign-up](https://img.shields.io/badge/no%20sign--up-free-22c55e) ![18+](https://img.shields.io/badge/18%2B-only-red) ![License](https://img.shields.io/badge/license-MIT-blue)

</div>

---

## What is Randomly?

Randomly is a free **random chat website** where you can **talk to strangers** by video or text, much like Omegle, Chatroulette and Ome.tv. It is a single static page (`index.html`) with **no backend of its own**, so you can host a copy for free on **GitHub Pages**. Video, audio and chat go **peer-to-peer over WebRTC**.

Looking for an **Omegle alternative**, an **Omegle clone** you can self-host, or a **random video chat** example built with WebRTC and PeerJS? This is it.

## Features

- 🎥 **Random video chat** with strangers, skip any time
- 💬 **Anonymous text chat** with "Stranger is typing…" indicator
- 🎯 **Shared interests**: add up to 5 interests and get matched with people who like the same things, then fall back to anyone
- 🛡️ **Maximum IP privacy**: forces all traffic through a TURN relay so strangers never see your IP address (on by default)
- 🙈 **Blur until revealed**: stranger's video starts blurred, tap to reveal
- 🚩 **Report & block**: reported strangers are blocked on your device for the session
- ⌨️ **Esc to skip** and one-tap Skip / Stop / New controls
- 📱 **Mobile friendly**: responsive layout, camera switch, mute, camera off, full screen, safe-area support
- 🔞 **18+ gate** with clear terms before first use
- 👤 **No accounts, no cookies, nothing stored**
- 🔍 SEO-ready landing page with FAQ and structured data

## Privacy: can strangers see my IP address?

In normal WebRTC calls the two browsers exchange network addresses, which reveals each person's IP. Randomly is built to avoid that:

- **Relay-only mode (default).** The app sets `iceTransportPolicy: "relay"`, so your browser only offers **TURN relay addresses**. The stranger sees the relay's IP, never yours. This applies to video, audio and the chat channel.
- **Fail closed.** If the relay is unreachable, the app tells you and does **not** silently fall back to a direct connection. You must explicitly choose "Use direct connection".
- **No third-party data about peers.** Strangers only learn the random session ID used for blocking.

What it **cannot** hide:

- Your IP is still visible to the **TURN relay provider** and the **PeerJS signaling server** (and to GitHub and CDNs that serve the page).
- Direct mode (turned on by the user) exposes your IP to strangers.
- A stranger can still record or screenshot your video and audio.
- Use a VPN as well if you need stronger anonymity.

## How it works

```
Browser A ◄══ WebRTC (video, audio, chat) via TURN relay ══► Browser B
    │                                                          │
    └──── PeerJS cloud signaling (matchmaking slots) ──────────┘
Static files (index.html) ──► GitHub Pages
```

There is no server to run. Matchmaking uses **slot-based rendezvous** on the free PeerJS signaling cloud:

1. Each pool (mode + interest) has a fixed set of slot IDs, for example `rndly1-v-i1x2y3-7`.
2. A searcher **scans** the slots. If one is registered, it connects and sends a `pick` message.
3. If nobody is waiting, the searcher **hosts**: it registers a random free slot for a few seconds, then scans again. This alternation guarantees two searchers find each other.
4. The host acknowledges exactly one searcher (others get `busy`), then starts the WebRTC call.

Interest pools are tried first. After about 10 seconds the app also searches the general pool.

## Deploy your own copy on GitHub Pages

1. Fork or create a repo and add `index.html`, `logo.svg`, `robots.txt` and `sitemap.xml` to the root.
2. Go to **Settings → Pages**, set **Source** to **Deploy from a branch**, choose `main` and `/ (root)`, and save.
3. Your site appears at `https://<your-username>.github.io/<repo>/` within a minute or two.
4. Update the URLs in `index.html` (`canonical`, `og:url`), `robots.txt` and `sitemap.xml` if your address differs from `https://69yujirohanma.github.io/Omegle/`.

## Configuration

Edit the `CFG` block near the bottom of `index.html`:

| Setting | Purpose |
|---|---|
| `NS` | Prefix for matchmaking IDs. **Change it** to create your own isolated pool of users |
| `SLOTS` | Number of matchmaking slots per pool (default 16) |
| `TURN` | TURN relay servers used for Maximum IP privacy |
| `STUN` | STUN servers used only when the user turns privacy mode off |
| `PEER` | Optional own PeerJS server, e.g. `{host:'peer.example.com',port:443,path:'/',secure:true}` |
| `REPORT_URL` | Where the report dialog sends people for abuse reports |

### Set up your own TURN relay (recommended)

The file ships with a **shared public test relay** so it works out of the box, but shared free relays can be slow, rate limited or removed. For a reliable, private setup:

1. Create a free account with a TURN provider (for example Metered, Twilio Network Traversal or a self-hosted `coturn`).
2. Copy the TURN URLs, username and credential into `CFG.TURN`.
3. Remember that anything in a static page is public. Use a provider plan with usage limits, and rotate credentials if they are abused.

## Limitations

- **No moderation.** There is no server, so nobody can review chats or ban users globally. Blocking is per device and per session.
- **No online counter** and no global statistics, for the same reason.
- **Slot squatting:** a bad actor could occupy matchmaking slots to disrupt matching. Change `NS` or run your own PeerJS server if that happens.
- **Not included from the original Omegle:** "college student" mode (needs email verification) and "spy / question" mode (needs a server). 
- Relay-only mode needs a working TURN server and uses relay bandwidth.
- Quality depends on both users' networks.

## Safety

Random chat with strangers carries real risks. Randomly is for **adults aged 18 and over only**. Never share personal information, and never do anything on camera you would not want recorded. Chats are unmoderated and may include offensive content. Report illegal content and any risk to children to local authorities or the National Center for Missing & Exploited Children (NCMEC). If you host a public copy, you are responsible for how it is used and for complying with the laws that apply to you.

## FAQ

**Is Randomly really an Omegle alternative?** It offers the same core idea: random text and video chat with strangers, with optional interests and no account. It is a small open-source project without moderation tools.

**Is it free?** Yes. It is free to use and free to host on GitHub Pages.

**Do I need to install anything or sign up?** No. It runs in current Chrome, Edge, Firefox and Safari on desktop and mobile.

**Can it work with only GitHub Pages?** Yes. Pages serves the static site, and the free PeerJS cloud handles signaling. A TURN relay is needed for the IP privacy mode.

**Does it store or record chats?** No. Nothing is recorded or stored by the site.

## Contributing

Issues and pull requests are welcome. Ideas: word filter, nickname or country tags, image-moderation hooks, own-signaling option, translations, accessibility improvements.

## License

MIT. Add a `LICENSE` file, or change this line, to match the license you choose.

## Disclaimer

Randomly is an independent open-source project and is **not affiliated with, endorsed by or connected to Omegle, Chatroulette, Ome.tv or any similar service**. Those names are trademarks of their respective owners. Provided "as is" without warranty.

---

<sub>**Keywords:** Omegle alternative, Omegle clone, free Omegle, random video chat, talk to strangers, anonymous chat, text chat with strangers, stranger chat, Chatroulette alternative, Ome.tv alternative, random chat no sign up, WebRTC video chat, PeerJS, GitHub Pages, open source random chat, IP address protection, TURN relay.</sub>
