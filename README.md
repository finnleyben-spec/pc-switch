# PC Switch

Eine kleine PWA, um einen PC per **Home Assistant** an- und auszuschalten —
komplett vom Handy, ohne Server auf dem PC und ohne offene Ports.

```
Handy (PWA)  ──WebSocket──▶  Home Assistant  ──▶  Entities:
  zwei Buttons               (Brücke, WS-API)     Wake (Wake-on-LAN) + Shutdown
```

- **An** → drückt die konfigurierte Wake-Entity (z. B. `button.mein_wake`)
- **Aus** → drückt die konfigurierte Shutdown-Entity (z. B. `input_button.pc_ausschalten`)

Beide Entities sind **frei konfigurierbar** im Setup der App — alles, was du
einträgst (HA-Adresse, Token, Entities), wird nur lokal im Browser des Geräts
gespeichert und verlässt das Handy nirgendwohin.

## Voraussetzungen auf Home Assistant

1. **Wake-Entity**: z. B. das [Wake on LAN](https://www.home-assistant.io/integrations/wake_on_lan/)-
   Addon/Integration als `button.*`-Entity.
2. **Shutdown-Entity**: eine `input_button`, die über Automation den PC
   herunterfährt (z. B. `remote.send_command`, `shell`-Service, Tailscale
   SSH, …) — wie immer du es bereits in HA gelöst hast.
3. **Long-Lived Access Token**: Profil → „Zugangs-Token“ → „Neues Token erstellen“.
4. Erreichbarkeit der HA-Adresse vom Handy (Lan, Tailscale oder ein
   TLS-Reverse-Proxy).

## Nutzung

1. Seite öffnen (GitHub Pages-URL, s. Repo).
2. Im Setup: HA-Adresse, Token, beide Entities eintragen → *Speichern & Verbinden*.
3. In den Browsermenüs „Zum Startbildschirm hinzufügen“ → App-Icon wie eine
   Native App.

### Adresse-Format

| Du gibst ein | App verbindet mit |
|---|---|
| `192.168.0.205:8123` | `ws://…` (nur im LAN, auf HTTPS-Seiten blockiert) |
| `https://ha.mein-netz.ts.net` | `wss://…` (Tailscale + TLS, auch von unterwegs) |
| `wss://ha.example.com` | 1:1 |

> **Hinweis:** GitHub Pages läuft auf HTTPS — dort blockiert der Browser
> unsichere `ws://`-Verbindungen. Für Nutzung von unterwegs also eine
> `https://`-HA-Adresse eintragen (z. B. Tailscale mit
> `share_homeassistant` im [Tailscale-Addon](https://github.com/hassio-addons/app-tailscale)).

## Entwicklung

Statisch, ein `index.html` in `www/`. Kein Build, kein Backend.

```bash
cd www && python3 -m http.server 8899   # http://127.0.0.1:8899
```

## Sicherheit

- Token und Entities: nur `localStorage` des jeweiligen Geräts.
- Kein Tracking, keine externen Abhängigkeiten, keine Telemetrie.
- Die PWA spricht ausschließlich die offizielle Home-Assistant-WebSocket-API.
