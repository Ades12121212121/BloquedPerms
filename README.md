# 📖 BloquedPerms Wiki v1.12

> Sistema de Bloqueo de Recetas con Tienda GUI para Minecraft

**Autor:** PCR087  
**Spigot:** https://www.spigotmc.org/resources/130781/  
**Versión Actual:** 1.12

---

## 📥 1. Instalación

1. Descarga `BloquedPerms-1.12.jar`
2. Colócalo en la carpeta `/plugins/`
3. Reinicia el servidor

### Dependencias:

| Tipo          | Plugin            | Uso                |
| ------------- | ----------------- | ------------------ |
| ✅ Requerido  | LuckPerms         | Manejo de permisos |
| ⭕ Opcional   | Vault + Economía  | Dinero             |
| ⭕ Opcional   | PlayerPoints      | Puntos (v1.8)      |
| ⭕ Opcional   | TokenManager      | Tokens (v1.8)      |
| ⭕ Opcional   | PlaceholderAPI    | Placeholders       |
| ⭕ Opcional   | WorldEdit         | Regiones (v1.10)   |
| ⭕ Compatible | ViaVersion 5.6.0+ | Multiversión       |

---

## 📁 2. Estructura de Carpetas

```
plugins/BloquedPerms/
├── config.yml          # Configuración principal
├── lang.yml            # Mensajes personalizables
├── purchases.yml       # Datos de compras (v1.4+)
├── temp_unlocks.yml    # Desbloqueos temporales (v1.6+)
└── Gui/
    ├── categorias.yml  # Menú de categorías
    ├── herramientas.yml
    ├── armas.yml
    ├── armaduras.yml
    ├── netherite.yml
    └── example.yml
```

---

## ⚙️ 3. Configuración Principal (config.yml)

```yaml
settings:
  prefix: "&8[&bBloquedPerms&8] "
  defaultGui: "categorias"
  debug: false
  prevent_crash_viaversion: true # v1.5 - Fix ViaVersion
  check_updates: true # v1.5 - Notificar actualizaciones

permissions:
  baseNode: "bloquedperms"
  adminBypass: "bloquedperms.bypass"
```

---

## 🚫 4. Bloquear Items (config.yml → blocked)

```yaml
blocked:
  iron_pickaxe:
    material: "IRON_PICKAXE"
    blockCraft: true # Bloquear crafteo
    blockUse: false # Bloquear uso
    blockSmithing: false # Bloquear mejora en smithing table
    category: "herramientas"
    requiredPerm: "crafteos.herramientas.iron_pickaxe"
    deny:
      messages:
        - "&c⛔ BLOQUEADO | No has desbloqueado &fIRON_PICKAXE"
        - "&7Usa &e/recetas herramientas &7para comprar"
      sound: "ENTITY_VILLAGER_NO;1;1"
```

---

## 🛒 5. Crear una GUI (archivos en /Gui/)

### Configuración básica:

```yaml
gui:
  title: "&f&lHerramientas"
  rows: 4
  openSound: "UI_BUTTON_CLICK"
  pagesEnabled: false
  totalPages: 3
  command: "herramientas"
  aliases: ["tools"]
  permission: "bloquedperms.shop"
```

### Items de compra:

```yaml
items:
  diamond_pickaxe:
    slot: 10
    material: "DIAMOND_PICKAXE"
    name: "&b&lPico de Diamante"
    lore:
      - "&7"
      - "&7Desbloquea el crafteo del"
      - "&7Pico de Diamante."
      - "&7"
      - "&fPrecio: &e$1000"
      - "&a▶ Click para comprar"
    price: 1000
    unlockId: "diamond_pickaxe"
    glow: true

    # Precio Dinámico (v1.4)
    dynamicPricing: true
    priceIncrement: 50
    priceMultiplier: 1.0
    maxPrice: 5000

    # Desbloqueo Temporal (v1.6)
    duration: "24h" # null = permanente
```

### Fillers (decoración):

```yaml
fillers:
  border:
    slots: [0, 1, 2, 3, 4, 5, 6, 7, 8, 27, 28, 29, 30, 31, 32, 33, 34, 35]
    material: "GRAY_STAINED_GLASS_PANE"
    name: " "
```

### Navegación:

```yaml
navigation:
  back:
    slot: 27
    material: "ARROW"
    name: "&c&l◄ Volver"
    action: "OPEN_GUI:categorias"

  next_page:
    slot: 53
    material: "ARROW"
    name: "&a&lSiguiente ▶"
    action: "NEXT_PAGE"
    disabledMaterial: "GRAY_STAINED_GLASS_PANE"
    disabledName: "&7No hay más páginas"
```

---

## 💰 6. Precios Dinámicos (v1.4)

Los precios suben según la demanda del servidor.

```yaml
items:
  diamond_pickaxe:
    price: 1000 # Precio base
    dynamicPricing: true # Activar precio dinámico
    priceIncrement: 50 # +$50 por cada compra global
    priceMultiplier: 1.0 # Multiplicador (1.05 = +5% por compra)
    maxPrice: 5000 # Precio máximo
```

**Fórmula:** `precio = base + (compras_globales * increment * multiplier)`

---

## 🏷️ 7. Descuentos por Grupo (v1.5)

```yaml
discounts:
  enabled: true
  stackDiscounts: false # true = acumular descuentos
  maxDiscount: 50 # Máximo 50% de descuento

  tiers:
    vip:
      permission: "bloquedperms.discount.vip"
      group: "vip"
      discount: 10
      priority: 1
      displayName: "&aVIP"

    mvp:
      permission: "bloquedperms.discount.mvp"
      group: "mvp"
      discount: 20
      priority: 2
      displayName: "&bMVP"

    ultra:
      permission: "bloquedperms.discount.ultra"
      group: "ultra"
      discount: 30
      priority: 3
      displayName: "&5ULTRA"

    legend:
      permission: "bloquedperms.discount.legend"
      group: "legend"
      discount: 40
      priority: 4
      displayName: "&6✦ LEGEND"
```

### Dar descuento a un jugador:

```bash
# Por permiso:
/lp user Jugador permission set bloquedperms.discount.vip

# Por grupo:
/lp user Jugador parent add vip
```

---

## ⏱️ 8. Desbloqueos Temporales (v1.6)

Los desbloqueos pueden tener duración limitada.

```yaml
items:
  diamond_pickaxe_temp:
    slot: 11
    material: "DIAMOND_PICKAXE"
    name: "&b&lPico de Diamante &7(24h)"
    price: 500
    unlockId: "diamond_pickaxe"
    duration: "24h" # Expira en 24 horas
```

### Formatos de Duración:

| Formato | Ejemplo | Descripción |
| ------- | ------- | ----------- |
| `Xw`    | `1w`    | X semanas   |
| `Xd`    | `3d`    | X días      |
| `Xh`    | `12h`   | X horas     |
| `Xm`    | `30m`   | X minutos   |

### Ejemplos:

```
"30m"  → 30 minutos
"12h"  → 12 horas
"24h"  → 1 día
"3d"   → 3 días
"7d"   → 1 semana
"1w"   → 1 semana
```

---

## 🌳 9. Requisitos Previos (v1.7)

Un item puede requerir que otro esté desbloqueado primero.

```yaml
requirements:
  # Para comprar pico de diamante, necesitas pico de hierro
  diamond_pickaxe:
    requires: ["iron_pickaxe"]
    displayName: "&bPico de Diamante"

  # Para netherite, necesitas diamante
  netherite_pickaxe:
    requires: ["diamond_pickaxe"]
    displayName: "&4Pico de Netherita"

  # Múltiples requisitos
  diamond_chestplate:
    requires: ["iron_helmet", "iron_chestplate", "iron_leggings", "iron_boots"]
    displayName: "&bPechera de Diamante"
```

### Árbol de Progresión:

```
NETHERITE PICKAXE
       ↑
 DIAMOND PICKAXE
       ↑
  IRON PICKAXE
       ↑
 STONE PICKAXE
```

---

## 💰 10. Múltiples Monedas (v1.8)

Ahora puedes usar diferentes tipos de moneda para cada item.

```yaml
items:
  # Item que cuesta DINERO (Vault)
  diamond_pickaxe:
    slot: 10
    material: "DIAMOND_PICKAXE"
    price: 1000
    unlockId: "diamond_pickaxe"
    currency: "vault" # Dinero ($1000)

  # Item que cuesta PUNTOS (PlayerPoints)
  diamond_sword:
    slot: 11
    material: "DIAMOND_SWORD"
    price: 500
    unlockId: "diamond_sword"
    currency: "points" # 500 puntos

  # Item que cuesta TOKENS (TokenManager)
  netherite_pickaxe:
    slot: 12
    material: "NETHERITE_PICKAXE"
    price: 100
    unlockId: "netherite_pickaxe"
    currency: "tokens" # 100 tokens
```

### Tipos de Moneda:

| Valor    | Plugin           | Descripción               |
| -------- | ---------------- | ------------------------- |
| `vault`  | Vault + Economía | Dinero (EssentialsX, CMI) |
| `money`  | Vault + Economía | Alias de vault            |
| `points` | PlayerPoints     | Puntos de jugador         |
| `tokens` | TokenManager     | Tokens                    |

---

## 🎮 11. Niveles de XP + Modo Economía (v1.9)

Ahora puedes elegir el modo de economía y cobrar niveles de XP.

### Modo de Economía:

```yaml
# config.yml

economy:
  # "classic" - Solo Vault (compatibilidad)
  # "advanced" - Múltiples monedas + XP
  mode: "advanced"

  showDetailedPrice: true
  allowXpCost: true
```

### Cobrar XP:

```yaml
items:
  # Item que cuesta dinero + XP
  netherite_pickaxe:
    slot: 10
    material: "NETHERITE_PICKAXE"
    price: 2000
    unlockId: "netherite_pickaxe"
    xpCost: 10 # Cuesta 10 niveles de XP

  # Item que SOLO cuesta XP
  special_item:
    slot: 11
    material: "NETHER_STAR"
    price: 0 # Sin costo de dinero
    unlockId: "special_item"
    xpCost: 30 # Solo cuesta XP
```

### Diferencias entre Modos:

| Característica    | Classic  | Advanced |
| ----------------- | -------- | -------- |
| Vault (dinero)    | ✅       | ✅       |
| PlayerPoints      | ❌       | ✅       |
| TokenManager      | ❌       | ✅       |
| XP como precio    | ❌       | ✅       |
| currency por item | Ignorado | ✅       |
| xpCost por item   | Ignorado | ✅       |

---

## 🗂️ 12. Regiones de Comandos (v1.10)

Restringe comandos a zonas específicas del mapa.

### Configuración en config.yml:

```yaml
regions:
  enabled: true
  blockOutsideRegion: true
  outsideMessage: "&c✖ Este comando solo puede usarse en zonas específicas."
```

### Archivo de Región (regions/spawn.yml):

```yaml
displayName: "&a&lSpawn"
world: "world"

pos1:
  x: -50
  y: 0
  z: -50

pos2:
  x: 50
  y: 256
  z: 50

priority: 10

# Comandos que SOLO funcionan en esta región
allowedCommands:
  - "shop"
  - "tienda"
  - "recetas"
```

**Permiso bypass:** `bloquedperms.regions.bypass`

---

## 📦 13. Items como Precio (v1.10)

Cobra items del inventario como parte del precio.

```yaml
items:
  netherite_pickaxe:
    slot: 10
    material: "NETHERITE_PICKAXE"
    price: 2000
    unlockId: "netherite_pickaxe"
    itemCost:
      DIAMOND: 64
      NETHERITE_INGOT: 4

  # Solo items (sin dinero)
  special_item:
    price: 0
    unlockId: "special_item"
    itemCost:
      NETHER_STAR: 2
      EMERALD_BLOCK: 16
```

---

## 📂 14. Categorías (v1.2)

```yaml
categories:
  herramientas:
    name: "&f&lHerramientas"
    description: "Picos, palas, hachas y azadas"
    icon: "IRON_PICKAXE"
    guiId: "herramientas"

  armas:
    name: "&c&lArmas"
    description: "Espadas, arcos y ballestas"
    icon: "DIAMOND_SWORD"
    guiId: "armas"

  netherite:
    name: "&4&lNetherite"
    description: "Mejoras de Netherite"
    icon: "NETHERITE_INGOT"
    guiId: "netherite"
    permission: "bloquedperms.category.netherite"
```

---

## ⌨️ 15. Comandos

| Comando                | Descripción              | Permiso              |
| ---------------------- | ------------------------ | -------------------- |
| `/bloquedperms reload` | Recargar configuración   | `bloquedperms.admin` |
| `/bloquedperms shop`   | Abrir tienda por defecto | `bloquedperms.shop`  |
| `/bloquedperms info`   | Info del plugin          | -                    |
| `/recipes [gui]`       | Abrir GUI específica     | `bloquedperms.shop`  |
| `/recetas`             | Comando personalizado    | Configurable         |

---

## 🔑 16. Permisos

| Permiso                        | Descripción                        |
| ------------------------------ | ---------------------------------- |
| `bloquedperms.*`               | Acceso total (OP)                  |
| `bloquedperms.admin`           | Comandos de admin + notificaciones |
| `bloquedperms.shop`            | Usar tiendas (default: true)       |
| `bloquedperms.bypass`          | Saltarse todos los bloqueos        |
| `bloquedperms.regions.bypass`  | Saltarse restricciones de regiones |
| `bloquedperms.category.*`      | Acceso a todas las categorías      |
| `bloquedperms.discount.vip`    | Descuento VIP (10%)                |
| `bloquedperms.discount.mvp`    | Descuento MVP (20%)                |
| `bloquedperms.discount.ultra`  | Descuento ULTRA (30%)              |
| `bloquedperms.discount.legend` | Descuento LEGEND (40%)             |

---

## 📊 17. Placeholders (PlaceholderAPI) (v1.3)

| Placeholder                     | Descripción                |
| ------------------------------- | -------------------------- |
| `%bloquedperms_unlocked%`       | Items desbloqueados        |
| `%bloquedperms_locked%`         | Items bloqueados           |
| `%bloquedperms_total%`          | Total de items             |
| `%bloquedperms_progress%`       | Porcentaje (ej: 45%)       |
| `%bloquedperms_progress_bar%`   | Barra visual ■■■□□□        |
| `%bloquedperms_categories%`     | Número de categorías       |
| `%bloquedperms_unlocked_<cat>%` | Desbloqueados en categoría |
| `%bloquedperms_total_<cat>%`    | Total en categoría         |
| `%bloquedperms_progress_<cat>%` | Progreso en categoría      |

---

## 🎯 18. Acciones Especiales

| Acción                          | Descripción      |
| ------------------------------- | ---------------- |
| `action: "CLOSE"`               | Cerrar la GUI    |
| `action: "OPEN_GUI:categorias"` | Abrir otra GUI   |
| `action: "NEXT_PAGE"`           | Siguiente página |
| `action: "PREV_PAGE"`           | Página anterior  |

---

## 💬 19. Mensajes (lang.yml)

```yaml
messages:
  # Bloqueo
  noPermissionCraft: "&c✖ No puedes craftear esto."
  noPermissionUse: "&c✖ No puedes usar esto."

  # Compra
  purchaseSuccess: "&a✔ Has desbloqueado &f%id% &apor &e$%price%"
  notEnoughMoney: "&c✖ No tienes suficiente dinero. Necesitas &e$%price%"
  alreadyOwned: "&e⚠ Ya tienes este desbloqueo."

  # Temporales (v1.6)
  temporaryPurchaseSuccess: "&a✔ Has desbloqueado &f%id% &7(Duración: &b%duration%&7)"
  temporaryExpired: "&e⚠ Tu desbloqueo de &f%id% &eha expirado."

  # Requisitos (v1.7)
  requirementsMissing: "&c✖ Primero debes desbloquear: &f%requirements%"

  # Múltiples Monedas (v1.8)
  currencyNotAvailable: "&c✖ La moneda &f%currency% &cno está disponible."
  notEnoughCurrency: "&c✖ No tienes suficiente %currency%. Necesitas &e%price%&c."
  # XP (v1.9)
  notEnoughXp: "&c✖ No tienes suficiente XP. Necesitas &b%required% niveles &c(tienes &f%current%&c)."
  purchaseSuccessWithXp: "&a✔ Has desbloqueado &f%id% &apor &e%price% &7+ &b%xp% niveles"
```

---

## 🔧 18. Solución de Problemas

### Crash con ViaVersion:

```yaml
settings:
  prevent_crash_viaversion: true
```

### Desactivar actualizaciones:

```yaml
settings:
  check_updates: false
```

### El item no se desbloquea:

1. Verifica que `unlockId` coincide con el ID en `blocked`
2. Verifica que LuckPerms está instalado
3. Activa `debug: true` y revisa logs

### El descuento no se aplica:

1. Verifica el permiso o grupo del jugador
2. Revisa `discounts.enabled: true`
3. Verifica la prioridad de los descuentos

---

## ✔️ 19. Confirmación de Compra (v1.11)

GUI de confirmación antes de comprar para evitar errores.

```yaml
confirmation:
  enabled: true
  minPrice: 500 # Solo confirmar compras ≥ $500
  title: "&c&l¿Confirmar Compra?"
```

### Funcionamiento:

1. Jugador hace click en item
2. Si precio ≥ minPrice → Se abre GUI de confirmación
3. Preview del item con precio, XP e items requeridos
4. Click en ✔ CONFIRMAR → Procesa compra
5. Click en ✖ CANCELAR → Vuelve a tienda
6. Expira después de 5 minutos

---

## 📜 22. Historial de Versiones

| Versión | Característica Principal                 |
| ------- | ---------------------------------------- |
| v1.11   | Confirmación de Compra                   |
| v1.10   | Regiones de Comandos + Items como Precio |
| v1.9    | Niveles de XP + Modo Economía            |
| v1.8    | Múltiples Monedas                        |
| v1.7    | Requisitos Previos                       |
| v1.6    | Desbloqueos Temporales                   |
| v1.5    | Descuentos por Grupo + ViaVersion Fix    |
| v1.4    | Precios Dinámicos                        |
| v1.3    | PlaceholderAPI                           |
| v1.2    | Categorías                               |
| v1.1    | Sistema de Páginas                       |
| v1.0    | Versión inicial                          |

---

## 📜 20. Historial de Compras (v1.12)

Consulta todas tus compras anteriores.

```yaml
history:
  enabled: true
  maxPerPlayer: 50
  showInChat: true
```

### Comando:

```
/bloquedperms history [página]
```

### Datos guardados:

- ID y nombre del item
- Precio y tipo de moneda
- XP gastado
- Fecha y mundo

---

## 📜 23. Historial de Versiones

| Versión | Característica Principal                 |
| ------- | ---------------------------------------- |
| v1.12   | Historial de Compras                     |
| v1.11   | Confirmación de Compra                   |
| v1.10   | Regiones de Comandos + Items como Precio |
| v1.9    | Niveles de XP + Modo Economía            |
| v1.8    | Múltiples Monedas                        |
| v1.7    | Requisitos Previos                       |
| v1.6    | Desbloqueos Temporales                   |
| v1.5    | Descuentos por Grupo + ViaVersion Fix    |
| v1.4    | Precios Dinámicos                        |
| v1.3    | PlaceholderAPI                           |
| v1.2    | Categorías                               |
| v1.1    | Sistema de Páginas                       |
| v1.0    | Versión inicial                          |

---

## 🚀 24. Próximas Versiones (Roadmap)

### v1.13 - Devoluciones

- Sistema de reembolso parcial
- Configurable por item

### v2.0 - Animaciones de GUI

- Items que cambian de color
- Efectos visuales al comprar
- GUI responsiva en tiempo real

---

## 📞 Soporte

- **Spigot:** https://www.spigotmc.org/resources/130781/
- **Autor:** PCR087

---

_BloquedPerms v1.12 - Sistema de Bloqueo de Recetas_
