---
description: Swarm communication options for DroneBridge for ESP32
---

# Drone Swarm Control

DroneBridge for ESP32 supports two radio transport options for swarm deployments. Both are fully encrypted using AES-256-GCM, but they are optimized for different operational tradeoffs.

{% hint style="info" %}
Choose the transport based on your environment and scaling needs:

- Use WiFi infrastructure if you want higher throughput and already have access points available.
- Use ESP-NOW if you need a self-contained setup with longer range and no external infrastructure.
{% endhint %}

## WiFi Infrastructure Mode

This option runs over standard WiFi access points, including modern WiFi 6 hardware. No proprietary radio hardware is required.

It is well suited for structured environments where infrastructure is already in place or can be deployed at low cost. The higher throughput makes it a practical choice for larger telemetry loads and for testing autonomy algorithms where data rate matters.

### Characteristics

| Parameter | Typical value |
| --- | --- |
| Infrastructure | Off-the-shelf access points |
| Range | Up to about 200 m |
| Throughput | High |
| Link saturation | Later onset |

## ESP-NOW Mode

ESP-NOW is a connectionless protocol that runs directly between ESP32 modules. No access point is required.

This makes it a strong option for outdoor or infrastructure-free deployments. The tradeoff is lower data throughput and earlier link saturation as the swarm grows.

### Characteristics

| Parameter | Typical value |
| --- | --- |
| Infrastructure | ESP32 modules only |
| Range | Up to about 1 km |
| Throughput | Lower |
| Link saturation | Earlier onset |

## Security and Shared Settings

For ESP-NOW swarm operation:

- All nodes must use the same WiFi password because it is used as the shared key.
- All nodes must use the same radio channel.

## Which Mode Should You Choose?

- Choose WiFi infrastructure mode for test environments, labs, and deployments where access points are easy to provide.
- Choose ESP-NOW for field use, longer range, and setups where you want to avoid extra infrastructure.
- If you expect many nodes or higher telemetry volume, WiFi infrastructure mode is usually the better starting point.
- If simplicity of deployment in open outdoor areas matters most, ESP-NOW is usually the better fit since no dedicated access point is required. In general the WiFi based setup is easier, more robust and offers the highest data rates and capacity.
