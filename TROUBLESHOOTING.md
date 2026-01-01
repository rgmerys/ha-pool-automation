# Troubleshooting Guide / Guía de Solución de Problemas

## Common Issues / Problemas Comunes

### 1. Error: "undefined is not an object (evaluating 't.includes')"

**Problem / Problema:**
This error appears in the automation description when viewing traces.

Este error aparece en la descripción de la automatización al ver las trazas.

**Cause / Causa:**
This is a frontend bug when using legacy trigger syntax (`platform:` instead of `trigger:`).

Es un bug del frontend al usar sintaxis antigua de triggers (`platform:` en lugar de `trigger:`).

**Solution / Solución:**

Change all triggers in your automations from:
```yaml
trigger:
  - platform: state
    entity_id: input_select.piscina_bomba_modo
  - platform: time_pattern
    minutes: "/1"
```

To:
```yaml
trigger:
  - trigger: state
    entity_id: input_select.piscina_bomba_modo
  - trigger: time_pattern
    minutes: "/1"
```

Cambia todos los triggers en tus automatizaciones de `platform:` a `trigger:`.

---

### 2. Sensor shows "unavailable" / Sensor muestra "unavailable"

**Problem / Problema:**
`sensor.piscina_tiempo_restante_en_manual` shows as unavailable.

`sensor.piscina_tiempo_restante_en_manual` se muestra como no disponible.

**Possible Causes / Causas Posibles:**

1. **Missing entities / Entidades faltantes:**
   - Verify all input helpers exist
   - Check entity IDs match your devices

2. **Template error / Error de template:**
   - Check Home Assistant logs for template errors
   - Look for `TemplateError` messages

3. **Timezone issue / Problema de timezone:**
   - Most common error: `can't subtract offset-naive and offset-aware datetimes`

**Solution / Solución:**

Check logs first:
```
Settings → System → Logs
Search for: "template" or "piscina"
```

If you see timezone error, ensure your template includes:
```yaml
{% set ahora = now().replace(tzinfo=None) %}
{% set manual_desde_naive = manual_desde.replace(tzinfo=None) %}
```

Si ves error de timezone, asegúrate que tu template incluya las líneas anteriores.

---

### 3. Time remaining shows "unknown" / Tiempo restante muestra "unknown"

**Problem / Problema:**
Sensor exists but shows "unknown" instead of countdown.

El sensor existe pero muestra "unknown" en lugar de la cuenta regresiva.

**Causes / Causas:**

1. **Not in Manual mode / No está en modo Manual:**
   - Sensor only shows time in Manual mode
   - In other modes, it shows "-"

2. **Manual timestamp not set / Timestamp manual no configurado:**
   - `input_datetime.piscina_bomba_en_manual_desde` might be at default value (1970-01-01)

3. **Update automation disabled / Automatización de actualización deshabilitada:**
   - Check `[Piscina] Actualizar tiempo restante cada minuto` is enabled

**Solution / Solución:**

1. Switch to Manual mode
2. Check in Developer Tools → States:
   ```
   input_datetime.piscina_bomba_en_manual_desde
   ```
   Should show current date, not 1970-01-01

3. Enable the update automation:
   ```
   Settings → Automations & Scenes
   Find: [Piscina] Actualizar tiempo restante cada minuto
   Ensure it's enabled
   ```

---

### 4. Dates showing as 1/1/1970

**Problem / Problema:**
Dashboard shows 1/1/1970 for manual mode timestamp.

El dashboard muestra 1/1/1970 para el timestamp de modo manual.

**Cause / Causa:**
You're displaying the `input_datetime` directly instead of the template sensor.

Estás mostrando el `input_datetime` directamente en lugar del sensor template.

**Solution / Solución:**

In your dashboard, use:
```yaml
# WRONG / INCORRECTO
- entity: input_datetime.piscina_bomba_en_manual_desde
  name: En manual desde

# CORRECT / CORRECTO
- entity: sensor.piscina_en_manual_desde
  name: En manual desde
```

Change to the template sensor that formats the date properly.

Cambia al sensor template que formatea la fecha correctamente.

---

### 5. Pump doesn't follow schedule / Bomba no sigue el horario

**Problem / Problema:**
Pump doesn't turn on/off according to configured schedule.

La bomba no se enciende/apaga según el horario configurado.

**Checklist / Lista de verificación:**

1. **Mode is Automatic / Modo es Automático:**
   ```
   input_select.piscina_bomba_modo = "Automático"
   ```

2. **Pump is enabled / Bomba está habilitada:**
   ```
   input_boolean.piscina_bomba_habilitada = ON
   ```

3. **Schedules are correct / Horarios son correctos:**
   - Check `input_datetime.piscina_bomba_verano_inicio`
   - Check `input_datetime.piscina_bomba_verano_fin`
   - Verify start time is BEFORE end time

4. **Season is detected correctly / Temporada detectada correctamente:**
   ```
   sensor.piscina_temporada = "Verano" or "Fuera de temporada"
   ```

5. **Current time is within schedule / Hora actual está dentro del horario:**
   - If in summer, current time should be between summer start/end
   - If off-season, current time should be between winter start/end

**Debug / Depuración:**

1. Check automation traces:
   ```
   Settings → Automations & Scenes
   Find: [Piscina] Control bomba según modo
   Click → Traces → View latest run
   ```

2. Look for which condition failed

3. Enable automation debug mode:
   ```yaml
   # Add to automation
   trace:
     stored_traces: 20
   ```

---

### 6. Manual mode doesn't auto-detect / Modo manual no se detecta automáticamente

**Problem / Problema:**
When you manually toggle the pump, it doesn't switch to Manual mode.

Cuando cambias la bomba manualmente, no cambia a modo Manual.

**Cause / Causa:**
The detection automation is disabled or has wrong trigger settings.

La automatización de detección está deshabilitada o tiene configuración incorrecta de trigger.

**Solution / Solución:**

1. **Check automation is enabled:**
   ```
   Settings → Automations & Scenes
   Find: [Piscina] Detectar control manual de bomba
   Ensure it's enabled
   ```

2. **Verify trigger format:**
   ```yaml
   trigger:
     - trigger: state
       entity_id: switch.sonoff_1001f68f89_1  # Your pump switch
       for: "00:00:02"  # Must be string format HH:MM:SS
   ```

3. **Check entity ID matches your pump:**
   - Replace `switch.sonoff_1001f68f89_1` with your actual pump entity

---

### 7. Season not updating / Temporada no se actualiza

**Problem / Problema:**
`sensor.piscina_temporada` shows wrong season or "Configurar fechas".

`sensor.piscina_temporada` muestra temporada incorrecta o "Configurar fechas".

**Causes / Causas:**

1. **Dates not configured / Fechas no configuradas:**
   - `input_datetime.piscina_temporada_inicio` is empty
   - `input_datetime.piscina_temporada_fin` is empty

2. **Dates in wrong format / Fechas en formato incorrecto:**
   - Must be valid dates
   - System automatically adjusts to current year

**Solution / Solución:**

1. Set season start date:
   ```
   Settings → Devices & Services → Helpers
   Find: [Piscina] Inicio temporada verano
   Set to first day of swimming season (e.g., 01 October)
   ```

2. Set season end date:
   ```
   Find: [Piscina] Fin temporada verano
   Set to last day of swimming season (e.g., 31 March)
   ```

3. Dates will auto-adjust to current/next year

4. For seasons that cross New Year (e.g., Oct to Mar), the system handles it automatically

---

### 8. Template syntax errors / Errores de sintaxis de template

**Common Template Errors / Errores Comunes de Template:**

**Error:** `UndefinedError: 'None' has no attribute 'year'`

**Fix:**
```yaml
# WRONG
{% set manual_desde = states('input_datetime.piscina_bomba_en_manual_desde') | as_datetime %}
{% if manual_desde.year > 1970 %}

# CORRECT
{% set manual_desde = states('input_datetime.piscina_bomba_en_manual_desde') | as_datetime %}
{% if manual_desde is not none and manual_desde.year > 1970 %}
```

**Error:** `TypeError: can't subtract offset-naive and offset-aware datetimes`

**Fix:**
```yaml
# Add this before datetime math:
{% set ahora = now().replace(tzinfo=None) %}
{% set manual_desde_naive = manual_desde.replace(tzinfo=None) %}
```

---

### 9. Energy monitoring not working / Monitoreo de energía no funciona

**Problem / Problema:**
Energy sensors show unavailable or incorrect values.

Sensores de energía muestran unavailable o valores incorrectos.

**Solution / Solución:**

1. **Check entity IDs:**
   - Verify your energy monitoring device exists
   - Update entity IDs in piscina.yaml to match your device

2. **Device not reporting:**
   - Check device is online
   - Check firmware is updated
   - Verify integration is working

3. **Missing utility_meter:**
   - Utility meters require the source sensor to have `state_class: total_increasing`
   - Check your energy sensor has correct state_class

**Alternative if no energy monitoring:**

Comment out or remove utility_meter section:
```yaml
# utility_meter:
#   piscina_consumo_diario:
#     ...
```

---

### 10. Lights not turning on at sunset / Luces no se encienden al atardecer

**Problem / Problema:**
Pool lights don't turn on automatically at sunset.

Las luces de la piscina no se encienden automáticamente al atardecer.

**Checklist / Lista de verificación:**

1. **Lights enabled / Luces habilitadas:**
   ```
   input_boolean.piscina_luz_habilitada = ON
   ```

2. **Sun integration configured / Integración de sol configurada:**
   ```
   Settings → Devices & Services
   Verify "Sun" integration is active
   ```

3. **Location set correctly / Ubicación configurada correctamente:**
   ```
   Settings → System → General
   Verify latitude/longitude are correct
   ```

4. **Automation enabled:**
   ```
   Find: [Piscina] Luz automática sunset con duración
   Ensure it's enabled
   ```

5. **Check trigger:**
   ```yaml
   trigger:
     - trigger: sun
       event: sunset
   ```

**Test manually:**

1. Set offset to 0 minutes
2. Set duration to 2 minutes
3. Wait for sunset
4. Check automation traces

---

## Getting Help / Obtener Ayuda

If you've tried everything and still have issues:

Si has intentado todo y aún tienes problemas:

1. **Check logs:**
   ```
   Settings → System → Logs
   Look for errors related to "piscina" or "template"
   ```

2. **Enable debug logging:**
   ```yaml
   # Add to configuration.yaml
   logger:
     default: info
     logs:
       homeassistant.components.template: debug
       homeassistant.helpers.template: debug
   ```

3. **Test templates manually:**
   ```
   Developer Tools → Template
   Paste your template code
   Check for errors
   ```

4. **Check automation traces:**
   ```
   Settings → Automations & Scenes
   Select automation → Traces
   View execution details
   ```

5. **Community support / Soporte de la comunidad:**
   - Post in Home Assistant Community Forum
   - Include: HA version, error logs, automation traces
   - Be specific about what's not working

6. **GitHub Issues:**
   - Create an issue with detailed information
   - Include configuration (remove sensitive data)
   - Provide error logs

---

## Validation Checklist / Lista de Validación

Before asking for help, verify:

Antes de pedir ayuda, verifica:

- [ ] Home Assistant version is 2024.12 or newer
- [ ] All entity IDs updated to match your devices
- [ ] All automations are enabled
- [ ] No errors in Home Assistant logs
- [ ] Configuration check passes (no YAML errors)
- [ ] Restarted Home Assistant after changes
- [ ] Templates work in Developer Tools → Template
- [ ] All input helpers exist and are configured

---

**Need more help? / ¿Necesitas más ayuda?**

- 📖 [Documentation](../README.md)
- 💬 [Community Forum](https://community.home-assistant.io/)
- 🐛 [Report Bug](https://github.com/rmerys/ha-pool-automation/issues)
