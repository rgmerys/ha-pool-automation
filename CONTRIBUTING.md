# Contributing to HA Pool Automation

First off, thank you for considering contributing to this project! 🎉

## How Can I Contribute?

### Reporting Bugs

Before creating bug reports, please check existing issues to avoid duplicates.

**When reporting a bug, include:**

- Home Assistant version
- Python version (if applicable)
- Description of the issue
- Steps to reproduce
- Expected behavior
- Actual behavior
- Relevant logs (sanitize sensitive data)
- Configuration (sanitize sensitive data)

**Template:**

```markdown
**Home Assistant Version:** 2024.12.1
**Python Version:** 3.12

**Description:**
Brief description of the issue

**Steps to Reproduce:**
1. Go to '...'
2. Click on '....'
3. See error

**Expected Behavior:**
What you expected to happen

**Actual Behavior:**
What actually happened

**Logs:**
```
Paste relevant logs here
```

**Configuration:**
```yaml
# Paste relevant configuration (remove passwords/tokens)
```
```

### Suggesting Enhancements

Enhancement suggestions are welcome! Consider:

- Is this useful for most users?
- Does it fit the project scope?
- Can it be optional/configurable?

**Enhancement template:**

```markdown
**Feature Description:**
Clear description of the proposed feature

**Use Case:**
Why is this useful? When would you use it?

**Proposed Implementation:**
How could this work? (optional)

**Alternatives Considered:**
Other ways to achieve the same goal (optional)
```

### Pull Requests

**Process:**

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Make your changes
4. Test thoroughly
5. Commit with clear messages (`git commit -m 'Add amazing feature'`)
6. Push to your fork (`git push origin feature/AmazingFeature`)
7. Open a Pull Request

**PR Guidelines:**

- Describe what your PR does
- Reference related issues
- Include tests if applicable
- Update documentation
- Follow existing code style

**PR Template:**

```markdown
## Description
Brief description of changes

## Related Issues
Fixes #123

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
- [ ] Tested on Home Assistant version X.X
- [ ] All automations work correctly
- [ ] No errors in logs
- [ ] Updated documentation

## Screenshots (if applicable)
Add screenshots to show changes
```

## Code Style

### YAML

- Use 2 spaces for indentation (not tabs)
- Keep lines under 100 characters when possible
- Use comments to explain complex logic
- Group related items together

**Example:**

```yaml
# Good
automation:
  - id: descriptive_id
    alias: "[Component] Clear action name"
    mode: restart
    trigger:
      - trigger: state
        entity_id: sensor.example
    action:
      - service: switch.turn_on
        target:
          entity_id: switch.example

# Avoid
automation:
- id: auto1
  alias: automation
  trigger:
  - platform: state
    entity_id: sensor.example
  action:
  - service: switch.turn_on
    entity_id: switch.example
```

### Templates

- Use clear variable names
- Add comments for complex logic
- Handle edge cases (unavailable, unknown, none)
- Test with various inputs

**Example:**

```yaml
# Good
state: >
  {% set modo = states('input_select.piscina_bomba_modo') %}
  {% if modo == 'Manual' %}
    {% set manual_desde = states('input_datetime.piscina_bomba_en_manual_desde') | as_datetime %}
    {% if manual_desde is not none and manual_desde.year > 1970 %}
      # Calculate time remaining
      ...
    {% else %}
      Esperando...
    {% endif %}
  {% else %}
    -
  {% endif %}

# Avoid
state: >
  {% if states('input_select.piscina_bomba_modo')=='Manual' %}
    {{(states('input_datetime.piscina_bomba_en_manual_desde')|as_datetime+timedelta(hours=states('input_number.piscina_manual_timeout_horas')|float)-now()).total_seconds()}}
  {% endif %}
```

### Documentation

- Use clear, concise language
- Provide examples
- Keep both English and Spanish versions updated
- Update screenshots when UI changes

## Testing Checklist

Before submitting:

- [ ] Test in clean Home Assistant installation
- [ ] Verify all automations trigger correctly
- [ ] Check no errors in Home Assistant logs
- [ ] Test with different entity IDs
- [ ] Verify dashboard displays correctly
- [ ] Test all three operating modes
- [ ] Verify season detection
- [ ] Test manual override
- [ ] Confirm time calculations are correct
- [ ] Check energy monitoring (if available)
- [ ] Test light automation
- [ ] Verify notification messages
- [ ] Confirm templates handle edge cases

## Development Setup

1. **Fork and clone:**
   ```bash
   git clone https://github.com/yourusername/ha-pool-automation.git
   cd ha-pool-automation
   ```

2. **Create test environment:**
   - Install Home Assistant in development mode
   - Create test switches/sensors
   - Copy piscina.yaml to packages folder

3. **Test changes:**
   - Restart Home Assistant after changes
   - Check logs for errors
   - Test all affected automations
   - Verify dashboard updates

4. **Document changes:**
   - Update README.md
   - Update TROUBLESHOOTING.md if needed
   - Add comments to complex code

## Community Guidelines

- Be respectful and constructive
- Help newcomers
- Share knowledge and experiences
- Report issues clearly
- Acknowledge others' contributions

## Questions?

- Not sure where to start? Open a discussion!
- Need help? Ask in the issues section
- Want to chat? Join the discussion board

## Recognition

Contributors will be acknowledged in:
- README.md contributors section
- Release notes for significant contributions
- Project documentation

Thank you for contributing! 🙏

---

## Contribuyendo al Proyecto

¡Gracias por considerar contribuir a este proyecto! 🎉

### Cómo Puedo Contribuir?

**Reportando Bugs:**
- Verifica que no exista un issue similar
- Incluye versión de HA, logs y pasos para reproducir
- Sanitiza datos sensibles

**Sugerencias de Mejoras:**
- Describe claramente la funcionalidad
- Explica el caso de uso
- Considera si es útil para la mayoría

**Pull Requests:**
- Fork → Rama de feature → Cambios → Tests → PR
- Sigue el estilo de código existente
- Actualiza documentación
- Prueba exhaustivamente

### Guías de Estilo

**YAML:**
- 2 espacios de indentación
- Comentarios claros
- Variables descriptivas

**Templates:**
- Nombres de variables claros
- Manejo de casos edge
- Comentarios en lógica compleja

**Documentación:**
- Lenguaje claro y conciso
- Ejemplos prácticos
- Mantener versiones EN/ES actualizadas

### Checklist de Testing

Antes de enviar PR:
- ✓ Probado en instalación limpia
- ✓ Sin errores en logs
- ✓ Todos los modos funcionan
- ✓ Dashboard correcto
- ✓ Documentación actualizada

¡Gracias por contribuir! 🙏
