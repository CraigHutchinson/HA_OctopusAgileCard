# HA_OctopusAgileCard
Home Assistant cards for Octopus Agile rates

# Cards
## Agile Octopus - Electricity Import Prices p/kWh
![image](https://github.com/CraigHutchinson/HA_OctopusAgileCard/assets/54269136/e310b30f-a0eb-40d6-9f12-be05221bcac8)
```
type: custom:apexcharts-card
header:
  show: true
  title: Agile Octopus - Electricity Import Prices
series:
  - entity: >-
      event.octopus_energy_electricity_{{METER_SERIAL_NUMBER}}_{{MPAN_NUMBER}}_current_day_rates
    offset: '-0m'
    type: column
    unit: p/kWh
    stroke_width: -4
    float_precision: 2
    extend_to: now
    name: Prev.
    color: grey
    data_generator: |
      return entity.attributes.rates.filter( 
      (entry) => {
        return (new Date(entry.end) < Date.now()); }
      ).map((entry) => {
        return [new Date(entry.start), entry.value_inc_vat*100];
      });
  - entity: >-
      event.octopus_energy_electricity_{{METER_SERIAL_NUMBER}}_{{MPAN_NUMBER}}_current_day_rates
    offset: '-0m'
    type: column
    unit: p/kWh
    float_precision: 2
    stroke_width: -4
    extend_to: now
    name: Next
    color_threshold:
      - value: -50
        color: blue
      - value: 0.1
        color: green
      - value: 10
        color: orange
      - value: 25
        color: red
    data_generator: |
      return entity.attributes.rates.filter( 
      (entry) => {
        return (new Date(entry.end) >= Date.now()); 
      }).map((entry) => {
        return [new Date(entry.start), entry.value_inc_vat*100];
      });
  - entity: event.octopus_energy_electricity_{{METER_SERIAL_NUMBER}}_{{MPAN_NUMBER}}_next_day_rates
    offset: '-0m'
    type: column
    unit: p/kWh
    float_precision: 2
    stroke_width: -4
    name: Upcoming
    color_threshold:
      - value: -50
        color: blue
      - value: 0.1
        color: green
      - value: 10
        color: orange
      - value: 30
        color: red
    data_generator: |
      return entity.attributes.rates.map((entry) => {
        return [new Date(entry.start), entry.value_inc_vat*100];
      }).filter( 
      (entry) => { return (entry[0] < end); }
      );      
now:
  show: true
  label: now
yaxis:
  - min: ~0
    max: ~40
    decimals: 0
    align_to: 5
graph_span: 24h
span:
  start: minute
  offset: '-2h'
experimental:
  color_threshold: true

```
