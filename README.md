# 🏊 Home Assistant Pool Automation Package

[![Home Assistant](https://img.shields.io/badge/Home%20Assistant-2024.12+-blue.svg)](https://www.home-assistant.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://github.com/yourusername/ha-pool-automation/graphs/commit-activity)

A comprehensive pool automation package for Home Assistant that provides intelligent pump control with manual override, seasonal scheduling, automatic light management, and energy monitoring.

[English](#english) | [Español](#español)

---

## English

### 🌟 Features

- **🔄 Three Operating Modes**
  - **OFF:** Pump always off
  - **Manual:** Full manual control with automatic timeout
  - **Automatic:** Schedule-based operation with summer/winter seasons

- **📅 Seasonal Schedules**
  - Different pump schedules for summer vs off-season
  - Automatic season detection based on configurable dates
  - Auto-update dates to next year when season ends

- **⏱️ Smart Manual Mode**
  - Automatically detects manual pump control and switches mode
  - Returns to Automatic after configurable timeout (default: 24h)
  - Real-time countdown display in HH:MM format
  - No need for separate manual switches

- **💡 Automatic Pool Lighting**
  - Triggers at sunset + configurable offset
  - Configurable duration
  - Easy enable/disable toggle

- **⚡ Energy Monitoring**
  - Daily, monthly, and yearly consumption tracking
  - Real-time power, voltage, and amperage display
  - Integration with Home Assistant Energy Dashboard

### 📸 Screenshots

![Main Dashboard](docs/screenshots/Dashboard%20Piscina.png)
*Main dashboard with complete pool control*

### 📋 Requirements

- Home Assistant OS/Supervised/Core 2024.12+
- Smart switch for pool pump (Sonoff, Shelly, etc.)
- Smart switch for pool light
- Energy monitoring device (optional but recommended)

### 🚀 Quick Start

1. **Download** `piscina.yaml` from this repository

2. **Enable packages** in your `configuration.yaml`:
   ```yaml
   homeassistant:
     packages: !include_dir_named packages
   ```

3. **Create packages folder** if it doesn't exist:
   ```bash
   mkdir config/packages
   ```

4. **Copy** `piscina.yaml` to `config/packages/`

5. **Edit entity IDs** in `piscina.yaml` to match your devices:
   ```yaml
   # Find and replace these with your actual entity IDs:
   switch.sonoff_1001f68f89_1          # Your pump switch
   switch.modulo_luz_piscina           # Your light switch
   sensor.disyuntor_piscina_energy     # Your energy sensor
   sensor.disyuntor_piscina_power      # Your power sensor
   sensor.disyuntor_piscina_current    # Your current sensor
   sensor.disyuntor_piscina_voltage    # Your voltage sensor
   ```

6. **Restart** Home Assistant

7. **Add dashboard** (optional): Copy content from `dashboard.yaml` to your Lovelace configuration

### 📊 Dashboard Setup

Create a new dashboard or add to existing:

1. Go to **Settings → Dashboards**
2. Create new dashboard or edit existing
3. Add cards from `dashboard.yaml`
4. Customize as needed

Key entities to display:
- `input_select.piscina_bomba_modo` - Operating mode selector
- `sensor.piscina_temporada` - Current season
- `sensor.piscina_en_manual_desde` - Manual mode timestamp
- `sensor.piscina_tiempo_restante_en_manual` - Countdown timer
- Energy sensors for monitoring

### ⚙️ Configuration

After installation, configure these settings:

#### Season Dates
Set your swimming season dates. The system will automatically:
- Detect if today is in or out of season
- Apply the correct pump schedule
- Roll over to next year when season ends

#### Pump Schedules
- **Summer:** Full filtration schedule (e.g., 08:00 - 20:00)
- **Off-Season:** Reduced schedule (e.g., 10:00 - 14:00)

#### Manual Mode
- **Timeout (hours):** How long before returning to Automatic
- The system automatically detects when you manually toggle the pump

#### Light Control
- **Offset:** Minutes after sunset to turn on
- **Duration:** How long lights stay on (in minutes)

### 🔧 Troubleshooting

<details>
<summary><b>Sensor shows "unavailable"</b></summary>

1. Check all entity IDs exist and match your devices
2. View Home Assistant logs for template errors
3. Restart Home Assistant (not just reload)
4. Verify timezone handling in template sensors
</details>

<details>
<summary><b>Time remaining not updating</b></summary>

The sensor updates every minute. If it doesn't:
1. Check automation `[Piscina] Actualizar tiempo restante cada minuto` is enabled
2. Verify you're in Manual mode
3. Check that `input_datetime.piscina_bomba_en_manual_desde` has a valid date (not 1970-01-01)
</details>

<details>
<summary><b>Pump not following schedule</b></summary>

1. Verify mode is "Automático"
2. Check `input_boolean.piscina_bomba_habilitada` is ON
3. Verify current time is within configured schedule
4. Check automation traces for errors
5. Ensure season dates are configured correctly
</details>

<details>
<summary><b>Error: "undefined is not an object"</b></summary>

This is a frontend display bug with legacy syntax. Update triggers from `platform:` to `trigger:`:
```yaml
# OLD
trigger:
  - platform: state

# NEW
trigger:
  - trigger: state
```
</details>

### 🛠️ Customization

#### Change Update Frequency
Modify the minute pattern in the update automation:
```yaml
trigger:
  - trigger: time_pattern
    minutes: "*"  # Every minute
    # minutes: "/5"  # Every 5 minutes
```

#### Add Notifications
Add to any automation:
```yaml
- service: notify.mobile_app_your_phone
  data:
    title: "Pool Automation"
    message: "Pump mode changed to Manual"
```

#### Variable Speed Pumps
If you have a variable speed pump, modify the service calls:
```yaml
- service: fan.set_percentage
  target:
    entity_id: fan.pool_pump
  data:
    percentage: 75
```

### 📝 Technical Notes

#### Why This Design?

**Manual Override Detection:** The system detects manual toggles and switches mode automatically. No need for separate manual controls.

**Timezone Handling:** Uses `.replace(tzinfo=None)` to normalize datetime objects before calculations, preventing "offset-naive and offset-aware" errors.

**Auto-Update Schedule:** Templates with `now()` auto-update, but we add a helper automation to ensure updates every minute when in Manual mode.

**Season Rollover:** Automation triggers at 23:59:50 on last day of season to increment dates by one year automatically.

### 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

### 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

Free to use, modify, and distribute. Attribution appreciated but not required.

### 🙏 Acknowledgments

- Home Assistant community for excellent documentation
- Contributors who helped test and debug
- Users who provided feedback and improvements

### 📞 Support

- **Issues:** [GitHub Issues](https://github.com/rgmerys/ha-pool-automation/issues)
- **Home Assistant Community:** [Forum Thread](https://community.home-assistant.io/t/complete-pool-automation-package-seasonal-schedules-manual-override-energy-monitoring/969037)

---

## Español

### 🌟 Características

- **🔄 Tres Modos de Operación**
  - **OFF:** Bomba siempre apagada
  - **Manual:** Control manual total con timeout automático
  - **Automático:** Operación según horarios con temporadas verano/invierno

- **📅 Horarios por Temporada**
  - Horarios diferentes para verano vs fuera de temporada
  - Detección automática de temporada según fechas configurables
  - Auto-actualización de fechas al año siguiente cuando termina la temporada

- **⏱️ Modo Manual Inteligente**
  - Detecta automáticamente control manual de la bomba y cambia de modo
  - Vuelve a Automático después de timeout configurable (predeterminado: 24h)
  - Visualización de cuenta regresiva en tiempo real en formato HH:MM
  - No requiere switches manuales separados

- **💡 Iluminación Automática**
  - Se activa al atardecer + offset configurable
  - Duración configurable
  - Fácil habilitación/deshabilitación

- **⚡ Monitoreo de Energía**
  - Seguimiento de consumo diario, mensual y anual
  - Visualización en tiempo real de potencia, voltaje y amperaje
  - Integración con Panel de Energía de Home Assistant

### 📸 Capturas de Pantalla

![Dashboard Principal](docs/screenshots/Dashboard%20Piscina.png)
*Dashboard principal con control completo de piscina*

### 📋 Requisitos

- Home Assistant OS/Supervised/Core 2024.12+
- Switch inteligente para bomba de piscina (Sonoff, Shelly, etc.)
- Switch inteligente para luz de piscina
- Dispositivo de monitoreo de energía (opcional pero recomendado)

### 🚀 Inicio Rápido

1. **Descarga** `piscina.yaml` de este repositorio

2. **Habilita packages** en tu `configuration.yaml`:
   ```yaml
   homeassistant:
     packages: !include_dir_named packages
   ```

3. **Crea la carpeta packages** si no existe:
   ```bash
   mkdir config/packages
   ```

4. **Copia** `piscina.yaml` a `config/packages/`

5. **Edita los entity IDs** en `piscina.yaml` para que coincidan con tus dispositivos:
   ```yaml
   # Busca y reemplaza estos con tus entity IDs reales:
   switch.sonoff_1001f68f89_1          # Tu switch de bomba
   switch.modulo_luz_piscina           # Tu switch de luz
   sensor.disyuntor_piscina_energy     # Tu sensor de energía
   sensor.disyuntor_piscina_power      # Tu sensor de potencia
   sensor.disyuntor_piscina_current    # Tu sensor de corriente
   sensor.disyuntor_piscina_voltage    # Tu sensor de voltaje
   ```

6. **Reinicia** Home Assistant

7. **Agrega dashboard** (opcional): Copia el contenido de `dashboard.yaml` a tu configuración Lovelace

### 📊 Configuración del Dashboard

Crea un nuevo dashboard o agrega al existente:

1. Ve a **Configuración → Dashboards**
2. Crea nuevo dashboard o edita existente
3. Agrega tarjetas desde `dashboard.yaml`
4. Personaliza según necesites

Entidades clave para mostrar:
- `input_select.piscina_bomba_modo` - Selector de modo de operación
- `sensor.piscina_temporada` - Temporada actual
- `sensor.piscina_en_manual_desde` - Timestamp modo manual
- `sensor.piscina_tiempo_restante_en_manual` - Temporizador cuenta regresiva
- Sensores de energía para monitoreo

### ⚙️ Configuración

Después de la instalación, configura estos ajustes:

#### Fechas de Temporada
Define las fechas de tu temporada de baño. El sistema automáticamente:
- Detecta si hoy está dentro o fuera de temporada
- Aplica el horario correcto de la bomba
- Actualiza al año siguiente cuando termina la temporada

#### Horarios de Bomba
- **Verano:** Horario completo de filtración (ej: 08:00 - 20:00)
- **Fuera de Temporada:** Horario reducido (ej: 10:00 - 14:00)

#### Modo Manual
- **Timeout (horas):** Tiempo antes de volver a Automático
- El sistema detecta automáticamente cuando cambias la bomba manualmente

#### Control de Luz
- **Offset:** Minutos después del atardecer para encender
- **Duración:** Cuánto tiempo permanecen encendidas las luces (en minutos)

### 🔧 Solución de Problemas

<details>
<summary><b>El sensor muestra "unavailable"</b></summary>

1. Verifica que todos los entity IDs existan y coincidan con tus dispositivos
2. Revisa los logs de Home Assistant para errores de template
3. Reinicia Home Assistant (no solo recarga)
4. Verifica el manejo de timezone en los sensores template
</details>

<details>
<summary><b>Tiempo restante no se actualiza</b></summary>

El sensor se actualiza cada minuto. Si no lo hace:
1. Verifica que la automatización `[Piscina] Actualizar tiempo restante cada minuto` esté habilitada
2. Confirma que estás en modo Manual
3. Verifica que `input_datetime.piscina_bomba_en_manual_desde` tenga una fecha válida (no 1970-01-01)
</details>

<details>
<summary><b>La bomba no sigue el horario</b></summary>

1. Verifica que el modo sea "Automático"
2. Verifica que `input_boolean.piscina_bomba_habilitada` esté ON
3. Confirma que la hora actual esté dentro del horario configurado
4. Revisa las trazas de automatización para errores
5. Asegúrate de que las fechas de temporada estén configuradas correctamente
</details>

<details>
<summary><b>Error: "undefined is not an object"</b></summary>

Este es un bug de visualización del frontend con sintaxis antigua. Actualiza los triggers de `platform:` a `trigger:`:
```yaml
# VIEJO
trigger:
  - platform: state

# NUEVO
trigger:
  - trigger: state
```
</details>

### 🛠️ Personalización

#### Cambiar Frecuencia de Actualización
Modifica el patrón de minutos en la automatización de actualización:
```yaml
trigger:
  - trigger: time_pattern
    minutes: "*"  # Cada minuto
    # minutes: "/5"  # Cada 5 minutos
```

#### Agregar Notificaciones
Agrega a cualquier automatización:
```yaml
- service: notify.mobile_app_tu_telefono
  data:
    title: "Automatización Piscina"
    message: "Modo de bomba cambiado a Manual"
```

#### Bombas de Velocidad Variable
Si tienes una bomba de velocidad variable, modifica las llamadas de servicio:
```yaml
- service: fan.set_percentage
  target:
    entity_id: fan.bomba_piscina
  data:
    percentage: 75
```

### 📝 Notas Técnicas

#### ¿Por Qué Este Diseño?

**Detección de Override Manual:** El sistema detecta cambios manuales y cambia de modo automáticamente. No necesita controles manuales separados.

**Manejo de Timezone:** Usa `.replace(tzinfo=None)` para normalizar objetos datetime antes de cálculos, previniendo errores de "offset-naive and offset-aware".

**Actualización Automática:** Los templates con `now()` se actualizan automáticamente, pero agregamos una automatización auxiliar para asegurar actualizaciones cada minuto en modo Manual.

**Cambio de Temporada:** La automatización se activa a las 23:59:50 del último día de temporada para incrementar fechas automáticamente al año siguiente.

### 🤝 Contribuciones

¡Las contribuciones son bienvenidas! Por favor:

1. Haz fork del repositorio
2. Crea una rama de funcionalidad
3. Haz tus cambios
4. Prueba exhaustivamente
5. Envía un pull request

### 📄 Licencia

Licencia MIT - ver archivo [LICENSE](LICENSE) para detalles.

Libre para usar, modificar y distribuir. Atribución apreciada pero no requerida.

### 🙏 Agradecimientos

- Comunidad de Home Assistant por excelente documentación
- Contribuidores que ayudaron a probar y depurar
- Usuarios que proporcionaron retroalimentación y mejoras

### 📞 Soporte

- **Issues:** [GitHub Issues](https://github.com/rgmerys/ha-pool-automation/issues)
- **Comunidad Home Assistant:** [Hilo del Foro](https://community.home-assistant.io/t/complete-pool-automation-package-seasonal-schedules-manual-override-energy-monitoring/969037)

---

**⭐ Si este proyecto te resultó útil, considera darle una estrella en GitHub!**
