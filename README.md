# PC Switch

Native Android-App zum An- und Ausschalten eines PCs über **Home Assistant** —
vom Handy, ohne Server auf dem PC und ohne offene Ports.

```
Handy (APK)  ──WebSocket──▶  Home Assistant  ──▶  Entities:
  zwei Buttons               (Brücke, WS-API)     Wake (Wake-on-LAN) + Shutdown
```

- **An** → drückt die konfigurierte Wake-Entity (z. B. `button.mein_wake`)
- **Aus** → drückt die konfigurierte Shutdown-Entity (z. B. `input_button.pc_ausschalten`)
- **Connect to home** → Tailscale trennen + OpenVPN Connect starten (Home-VPN)

Beide Entities sind **frei konfigurierbar** im Setup der App — alles, was du
einträgst (HA-Adresse, Token, Entities), wird nur lokal auf dem Gerät gespeichert.

## Voraussetzungen auf Home Assistant

1. **Wake-Entity**: z. B. das [Wake on LAN](https://www.home-assistant.io/integrations/wake_on_lan/)-Addon/Integration als `button.*`-Entity.
2. **Shutdown-Entity**: eine `input_button`, die über Automation den PC herunterfährt.
3. **Long-Lived Access Token**: Profil → „Zugangs-Token“ → „Neues Token erstellen“.
4. Erreichbarkeit der HA-Adresse vom Handy (LAN, VPN oder Tailscale-Relay).

## Installation

[pc-switch-v2.0.0.apk](dist/pc-switch-v2.0.0.apk) herunterladen, auf dem Handy öffnen und installieren („Unbekannte App-Quellen“ erlauben).

Die App aktualisiert sich selbst (manuell, über den Update-Button in der App).

## Sicherheit

- Token und Entities: nur `SharedPreferences` auf dem Gerät.
- Kein Tracking, keine externen Abhängigkeiten, keine Telemetrie.
- Die App spricht ausschließlich die offizielle Home-Assistant-WebSocket-API.