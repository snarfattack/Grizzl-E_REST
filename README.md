# Grizzl-E_REST
Home Assistant REST Integration for Grizzl-E Ultimate 48A charger

Since they have put the OCPP integration behind a paywall, this is my workaround for local monitoring and control

Setting the current requires an input_number helper to hold the value, and then automation to call the rest command when the input_number value changes

