# OpenSearch Component for Home-Assistant

Publish Home Assistant events to your [OpenSearch](https://opensearch.org) cluster!

## Table of Contents

- [Getting Started](#getting-started)
- [Features](#features)
- [Inspiration](#inspiration)
- [Create Your Own Cluster Health Sensor](#create-your-own-cluster-health-sensor)
- [Support](#support)
- [Contributing](#contributing)

## Getting Started

The plugin has to be installed manually. To do so, follow these steps:

### 1. Download the Component

Download the latest release as a ZIP file.

### 2. Extract the Files

Extract the contents of the ZIP file to a folder.

### 3. Copy to Home Assistant Configuration Directory

Locate your Home Assistant configuration directory. This is typically the directory containing your `configuration.yaml` file.

Inside the configuration directory, create a folder named `custom_components` if it doesn't already exist.

Copy the extracted folder into the `custom_components` directory.

### 4. Restart Home Assistant

After copying the files, restart Home Assistant to load the new component.

### 5. Configure the Integration

Once restarted, add the integration via the Home Assistant UI:

- Navigate to **Settings** > **Devices & Services**.
- Click on **Add Integration**.
- Search for and select **OpenSearch**.
- Follow the on-screen instructions to complete the setup.

## Features

- Efficiently publishes Home-Assistant events to OpenSearch using the Bulk API
- Automatically sets up Datastreams using Time Series Data Streams ("TSDS"), Datastream Lifecycle Management ("DLM"), or Index Lifecycle Management ("ILM") depending on your cluster's capabilities
- Supports OpenSearch's security features via optional username, password, and API keys
- Selectively publish events based on domains or entities

## Inspiration

### HVAC Usage
Graph your home's climate and HVAC usage:

![img](docs/assets/hvac-history.png)

### Weather Station
Visualize and alert on data from your weather station:

![img](docs/assets/weather-station.png)

![img](docs/assets/weather-station-wind-pressure.png)

### Additional Examples

Some usage examples inspired by [real users](https://github.com/legrego/homeassistant-opensearch/issues/203):

- Utilize a Raspberry Pi in [kiosk mode](https://www.raspberrypi.com/tutorials/how-to-use-a-raspberry-pi-in-kiosk-mode/) with a 15" display to create rotating fullscreen [OpenSearch Dashboards](https://www.opensearch.org/docs/latest/dashboards/) displays. These displays can show metrics from various Home Assistant integrations, providing visually dynamic dashboards for monitoring smart home data.
- For temperature-sensitive appliances like refrigerators and freezers, temperature sensors send data to Home Assistant, which is published to OpenSearch. Kibana’s [alerting framework](https://www.elastic.co/kibana/alerting) can notify the user if temperatures deviate beyond the set limits.
- Monitor the humidity and temperature in a snake enclosure or habitat with OpenSearch alerting to track conditions over time. This solution is simpler and more intuitive than relying on Home Assistant automations alone.
- Users can maintain smaller data subsets for long-term analysis, which is especially useful for trends like weather data. This extended retention allows for more in-depth analysis over time, contrasting with Home Assistant’s typical data retention limits.

## Create Your Own Cluster Health Sensor

Older versions (prior to `0.6.0`) included a cluster health sensor, but this has been removed in favor of a more flexible approach. You can create your own cluster health sensor by using Home Assistant’s built-in [REST sensor](https://www.home-assistant.io/integrations/sensor.rest).

```yaml
# Example configuration
sensor:
  - platform: rest
    name: "Cluster Health"
    unique_id: "cluster_health" # Replace with your own unique id
    resource: "https://example.com/_cluster/health" # Replace with your OpenSearch URL
    username: hass # Replace with your username
    password: changeme # Replace with your password
    value_template: "{{ value_json.status }}"
    json_attributes: # Optional attributes you may want to include from the /_cluster/health API response
      - "cluster_name"
      - "status"
      - "timed_out"
      - "number_of_nodes"
      - "number_of_data_nodes"
      - "active_primary_shards"
      - "active_shards"
      - "relocating_shards"
      - "initializing_shards"
      - "unassigned_shards"
      - "delayed_unassigned_shards"
      - "number_of_pending_tasks"
      - "number_of_in_flight_fetch"
      - "task_max_waiting_in_queue_millis"
      - "active_shards_percent_as_number"
```

## Support

This project is not officially supported by OpenSearch or Home Assistant. Please open a GitHub issue for any questions, bugs, or feature requests.

## Contributing

We welcome contributions! Please check the [Contributing Guide](CONTRIBUTING.md) for more details.
