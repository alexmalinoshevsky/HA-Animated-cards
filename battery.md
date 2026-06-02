<!-- anashost_support_badges_start -->
[![Revolut.Me][revolut_me_shield]][revolut_me]
[![PayPal.Me][paypal_me_shield]][paypal_me]
[![ko_fi][ko_fi_shield]][ko_fi_me]
[![buymecoffee][buy_me_coffee_shield]][buy_me_coffee_me]
[![patreon][patreon_shield]][patreon_me]
<!-- anashost_support_badges_end -->
<!-- 
```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
-->

<div align="left">
<a href="https://github.com/Anashost/HA-Animated-cards">
  <img src="https://img.shields.io/badge/◀_BACK_TO_HOME_Page-2196F3?style=for-the-badge&logoColor=white" height="60" />
</a>
</div>

# Home Assistant Animated Battery / Percentage Cards

This [YouTube Video](https://youtu.be/SrFbC1ae35E) explains how to do it.

![gif-16c233439b5ca922](https://github.com/user-attachments/assets/ed91507c-2dc6-4c35-8825-0df2b2b72f69)
`Loading image... please wait`

<hr>

> [!NOTE]
> If you are using the **Sections** view type, you may need to set `rows` to around `1.5` for the card,
> otherwise the card may appear compressed.
> (USE THIS ONLY IF YOU HAVE ISSUES).
> 
> ```yaml
> grid_options:
>   rows: 1.5
> ```

<hr>

# Cards:

>Note: Each card has two versions:
>- Normal version: number and percentage.
>
>- Low/Medium/High only version.

<details>
<summary><strong>Battery card 1</summary>

```yaml
type: custom:mushroom-entity-card
entity: sensor.anasbox_battery_level
tap_action:
  action: more-info
icon: ""
icon_color: white
primary_info: name
secondary_info: none
name: AnasBox Phone
card_mod:
  style:
    .: |
      ha-card {
        /* ====== USER SETTINGS ===== */
        {% set charging_entity = 'binary_sensor.anasbox_is_charging' %}

        /* Optional: replace xx with a percentage to show soc */
        {% set target_soc = xx %}

        /* ======= FONT SETTINGS ======= */
        --card-primary-font-size: 15px !important;
        --card-secondary-font-size: 12px !important;
        --card-primary-font-weight: bold !important;
        /* ========================= */

        {% set level = states(config.entity) | float(0) %}
        {% set is_charging = is_state(charging_entity, 'on') %}
        
        {% if is_charging %}
           {% set color = '0, 255, 255' %}
        {% elif level <= 20 %}
          {% set color = '244, 67, 54' %}
        {% elif level <= 60 %}
          {% set color = '255, 152, 0' %}
        {% else %}
          {% set color = '0, 255, 100' %}
        {% endif %}

        --custom-level: {{ level }}%;
        --custom-color: rgba({{ color }}, 0.8);
        --custom-bubble: {{ 'block' if is_charging else 'none' }};
        --custom-icon-shadow: {{ '0 2px 10px rgba(' ~ color ~ ', 0.3)' if is_charging else 'none' }};

        position: relative;
        overflow: hidden;
        transition: all 0.5s ease;
        z-index: 1;
        
        {% if target_soc is defined and target_soc is number %}
        background-image: 
          radial-gradient(circle at 24px 24px, rgba({{ color }}, 0.15) 0%, transparent 60%),
          linear-gradient(to bottom, rgba(128, 128, 128, 0.6) 4px, transparent 4px) !important;
        background-size: 100% 100%, 2px 8px !important; 
        background-position: 0 0, {{ target_soc }}% 0 !important; 
        background-repeat: no-repeat, repeat-y !important;
        {% else %}
        background-image: 
          radial-gradient(circle at 24px 24px, rgba({{ color }}, 0.15) 0%, transparent 60%) !important;
        background-size: 100% 100% !important; 
        background-position: 0 0 !important; 
        background-repeat: no-repeat !important;
        {% endif %}
      }

      {% if target_soc is defined and target_soc is number %}
      ha-card::before {
        content: "{{ target_soc }}%";
        position: absolute !important;
        top: 60px !important;
        left: {{ target_soc }}% !important;
        
        {% if target_soc <= 10 %}
          transform: translateX(0%);
        {% elif target_soc >= 90 %}
          transform: translateX(-100%);
        {% else %}
          transform: translateX(-50%);
        {% endif %}
        
        background: var(--card-background-color, #fafafa);
        border: 1px solid rgba(128, 128, 128, 0.6);
        color: var(--primary-text-color);
        font-size: 10px;
        font-weight: bold;
        padding: 2px 8px;
        border-radius: 12px;
        z-index: 1;
        box-shadow: 0 2px 6px rgba(0,0,0,0.15);
        pointer-events: none;

        bottom: auto !important; 
        right: auto !important;
        margin: 0 !important;
        height: auto !important;
        width: auto !important;
      }
      {% endif %}

      ha-card::after {
        content: '';
        position: absolute !important;
        bottom: 0 !important; 
        left: 0 !important;
        height: 4px !important;
        width: {{ level }}% !important;
        background: linear-gradient(90deg, transparent, rgb({{ color }}));
        box-shadow: 0 0 10px rgba({{ color }}, 0.5);
        transition: width 0.5s ease;
        z-index: 3;
        pointer-events: none;
      }

      mushroom-state-item {
        position: static !important;
      }

      /* Light Theme (Default) */
      mushroom-state-item::after {
        content: '{{ level | round(0) }}%';
        position: absolute !important;
        top: 12px !important; 
        right: 12px !important;
        font-size: 1rem; font-weight: 700;
        text-shadow: 0 1px 3px rgba(0, 0, 0, 0.8);
        box-shadow: 0 2px 6px rgba(0,0,0,0.15);
        backdrop-filter: blur(4px);
        padding: 2px 6px; border-radius: 4px;
        z-index: 4;
        pointer-events: none;
        background: rgba(0, 0, 0, 0.5) !important;
        border: 1px solid rgba(255, 255, 255, 0.1) !important;
        color: rgb({{ color }}) !important;
      }

      mushroom-shape-icon {
        --icon-size: 65px;
        display: flex;
        padding-right: 15px;
        padding-bottom: 5px;
      }
      mushroom-state-info {
        position: relative;
        z-index: 3;
      }

    mushroom-shape-icon$: |
      .shape {
        --liquid-level: var(--custom-level);
        --liquid-color: var(--custom-color);
        --bubble-display: var(--custom-bubble);
        background: rgba(var(--rgb-primary-text-color, 0, 0, 0), 0.03) !important;
        overflow: hidden !important; 
        position: relative;
        border: 1px solid rgba(var(--rgb-primary-text-color, 0, 0, 0), 0.08);
        box-shadow: var(--custom-icon-shadow) !important;
      }

      .shape::before {
        content: '';
        position: absolute;
        left: -50%;
        width: 200%;
        height: 200%;
        top: calc(100% - var(--liquid-level));
        background: var(--liquid-color);
        border-radius: 40%; 
        animation: liquid-wave 6s linear infinite;
        opacity: 0.85;
      }

      .shape::after {
        content: '';
        display: var(--bubble-display);
        position: absolute;
        inset: 0;
        background-image: 
          radial-gradient(2px 2px at 20% 80%, rgba(255,255,255,0.9), transparent),
          radial-gradient(2px 2px at 50% 70%, rgba(255,255,255,0.9), transparent),
          radial-gradient(3px 3px at 80% 90%, rgba(255,255,255,0.9), transparent);
        background-size: 100% 100%;
        animation: bubbles-rise 0.7s linear infinite;
      }

      ha-icon {
        position: relative;
        z-index: 2;
        color: white !important;
      }

      @keyframes liquid-wave {
        0% { transform: rotate(0deg); }
        100% { transform: rotate(360deg); }
      }
      @keyframes bubbles-rise {
        0% { transform: translateY(10px); opacity: 0; }
        50% { opacity: 1; }
        100% { transform: translateY(-20px); opacity: 0; }
      }

```
</details>

<details>
<summary><strong>Battery card 1 (low, medium, high)</summary>

```yaml
type: custom:mushroom-entity-card
entity: sensor.anasbox_battery_level
tap_action:
  action: more-info
icon: ""
icon_color: white
primary_info: name
secondary_info: none
name: AnasBox Phone
card_mod:
  style:
    .: |
      ha-card {
        /* ====== USER SETTINGS ===== */
        {% set charging_entity = 'binary_sensor.anasbox_is_charging' %}

        /* ======= FONT SETTINGS ======= */
        --card-primary-font-size: 15px !important;
        --card-secondary-font-size: 12px !important;
        --card-primary-font-weight: bold !important;
        /* ========================== */

        {% set level_txt = states(config.entity) | lower %}
        {% set is_charging = is_state(charging_entity, 'on') %}
        
        {% if is_charging %}
           {% set color = '0, 255, 255' %}
        {% elif level_txt == 'low' %}
          {% set color = '244, 67, 54' %}
        {% elif level_txt == 'medium' %}
          {% set color = '255, 152, 0' %}
        {% elif level_txt == 'high' %}
          {% set color = '0, 255, 100' %}
        {% else %}
          {% set color = '74, 74, 74' %}
        {% endif %}

        {% set mapped_level = 20 if level_txt == 'low' else (50 if level_txt == 'medium' else 100) %}

        --custom-level: {{ mapped_level }}%;
        --custom-color: rgba({{ color }}, 0.8);
        --custom-bubble: {{ 'block' if is_charging else 'none' }};
        --custom-icon-shadow: {{ '0 2px 10px rgba(' ~ color ~ ', 0.3)' if is_charging else 'none' }};

        position: relative;
        overflow: hidden;
        transition: all 0.5s ease;
        z-index: 1;
        
        background-image: radial-gradient(circle at 24px 24px, rgba({{ color }}, 0.15) 0%, transparent 60%) !important;
        background-size: 100% 100% !important; 
        background-position: 0 0 !important; 
        background-repeat: no-repeat !important;
      }

      ha-card::after {
        content: '';
        position: absolute !important;
        bottom: 0 !important; 
        left: 0 !important;
        height: 4px !important;
        width: {{ mapped_level }}% !important;
        background: linear-gradient(90deg, transparent, rgb({{ color }}));
        box-shadow: 0 0 10px rgba({{ color }}, 0.5);
        transition: width 0.5s ease;
        z-index: 3;
        pointer-events: none;
      }

      mushroom-state-item {
        position: static !important;
      }

      mushroom-state-item::after {
        content: '{{ level_txt | capitalize }}';
        position: absolute !important;
        top: 12px !important; 
        right: 12px !important;
        font-size: 1rem; font-weight: 700;
        text-shadow: 0 1px 3px rgba(0, 0, 0, 0.8);
        box-shadow: 0 2px 6px rgba(0,0,0,0.15);
        backdrop-filter: blur(4px);
        padding: 2px 6px; border-radius: 4px;
        z-index: 4;
        pointer-events: none;
        background: rgba(0, 0, 0, 0.5) !important;
        border: 1px solid rgba(255, 255, 255, 0.1) !important;
        color: rgb({{ color }}) !important;
      }

      mushroom-shape-icon {
        --icon-size: 65px;
        display: flex;
        padding-right: 15px;
        padding-bottom: 5px;
      }

      mushroom-state-info {
        position: relative;
        z-index: 3;
      }
    mushroom-shape-icon$: |
      .shape {
        --liquid-level: var(--custom-level);
        --liquid-color: var(--custom-color);
        --bubble-display: var(--custom-bubble);
        
        background: rgba(var(--rgb-primary-text-color, 0, 0, 0), 0.03) !important;
        overflow: hidden !important; 
        position: relative;
        border: 1px solid rgba(var(--rgb-primary-text-color, 0, 0, 0), 0.08);
        box-shadow: var(--custom-icon-shadow) !important;
      }

      .shape::before {
        content: '';
        position: absolute;
        left: -50%;
        width: 200%;
        height: 200%;
        top: calc(100% - var(--liquid-level));
        background: var(--liquid-color);
        border-radius: 40%; 
        animation: liquid-wave 6s linear infinite;
        opacity: 0.85;
      }

      .shape::after {
        content: '';
        display: var(--bubble-display);
        position: absolute;
        inset: 0;
        background-image: 
          radial-gradient(2px 2px at 20% 80%, rgba(255,255,255,0.9), transparent),
          radial-gradient(2px 2px at 50% 70%, rgba(255,255,255,0.9), transparent),
          radial-gradient(3px 3px at 80% 90%, rgba(255,255,255,0.9), transparent);
        background-size: 100% 100%;
        animation: bubbles-rise 0.7s linear infinite;
      }

      ha-icon {
        position: relative;
        z-index: 2;
        color: white !important;
      }
      @keyframes liquid-wave {
        0% { transform: rotate(0deg); }
        100% { transform: rotate(360deg); }
      }
      @keyframes bubbles-rise {
        0% { transform: translateY(10px); opacity: 0; }
        50% { opacity: 1; }
        100% { transform: translateY(-20px); opacity: 0; }
      }

````
</details>

<details>
<summary><strong>Battery card 2</summary>

```yaml
type: custom:mushroom-entity-card
entity: sensor.anasbox_battery_level
tap_action:
  action: more-info
name: AnasBox Phone
icon_color: white
primary_info: state
secondary_info: name
card_mod:
  style: |
    ha-card {
      /* ====== USER SETTINGS ===== */
      {% set charging = is_state('binary_sensor.anasbox_is_charging', 'on') %}
      
      /* ======= FONT SETTINGS ======= */
      --card-primary-font-size: 15px !important;
      --card-secondary-font-size: 12px !important;
      --card-primary-font-weight: bold !important;
      /* ========================= */

      {% set level = states(config.entity) | float(0) %}
      
      {% if charging %}
        {% set rgb = '29, 130, 150' %}      /* Blue (Charging) */
      {% elif level <= 20 %}
        {% set rgb = '150, 29, 29' %}       /* Red */
      {% elif level <= 60 %}
        {% set rgb = '150, 109, 29' %}      /* Orange */
      {% else %}
        {% set rgb = '29, 150, 59' %}       /* Green */
      {% endif %}

      --liq-color: rgb({{ rgb }});
      --liq-level: {{ level }}%;
      --wave-speed: {{ '3s' if charging else '8s' }};
      
      border-radius: var(--ha-card-border-radius, 12px);
      position: relative;
      overflow: hidden;
      z-index: 0;
      transition: all 0.5s ease;
    }

    mushroom-shape-icon {
      --icon-size: 68px;
      display: flex;
      margin: -18px 0 10px -18px !important;
      padding-right: 15px;
      z-index: 2;
    }

    ha-card::before {
      content: "";
      position: absolute;
      top: 0; left: 0; bottom: 0;
      z-index: -1;
      
      width: calc(var(--liq-level) - 60px);
      
      {% if level >= 100 %}
        width: 100%;
      {% endif %}
      
      background: var(--liq-color);
      
      transition: width 0.5s cubic-bezier(0.25, 0.1, 0.25, 1);
    }

    ha-card::after {
      content: "";
      position: absolute;
      z-index: -1;
      
      display: {{ 'none' if level <= 0 or level >= 100 else 'block' }};
      
      width: 120px;
      height: 120px;
      
      background: var(--liq-color);
      box-shadow: 0 0 25px rgba({{ rgb }}, 0.5);
      border-radius: 40%;
      
      left: calc(var(--liq-level) - 120px); 
      top: calc(50% - 60px);
      
      animation: spin-wave var(--wave-speed) linear infinite;
      transition: left 0.5s cubic-bezier(0.25, 0.1, 0.25, 1);
    }

    .mushroom-state-item {
      z-index: 2;
      position: relative;
      /* Add shadow so text is readable over the bright liquid */
      text-shadow: 0 1px 3px rgba(0,0,0,0.8);
    }

    ha-state-icon {
      color: white !important;
      filter: drop-shadow(0 2px 4px rgba(0,0,0,0.5));
    }

    @keyframes spin-wave {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }

```
</details>

<details>
<summary><strong>Battery card 2 (low, medium, high)</summary>

```yaml
type: custom:mushroom-entity-card
entity: sensor.anasbox_battery_level
tap_action:
  action: more-info
name: AnasBox Phone
icon_color: white
primary_info: state
secondary_info: none
card_mod:
  style: |
    ha-card {
      /* ====== USER SETTINGS ===== */
      {% set charging = is_state('binary_sensor.anasbox_is_charging', 'on') %}

      /* ======= FONT SETTINGS ======= */
      --card-primary-font-size: 15px !important;
      --card-secondary-font-size: 12px !important;
      --card-primary-font-weight: bold !important;
      /* ========================= */
    
      {% set level_txt = states(config.entity) | lower %}
      {% set level = 20 if level_txt == 'low' else (50 if level_txt == 'medium' else 90) %}

      {% if charging %}
        {% set rgb = '29, 130, 150' %}
      {% elif level_txt == 'low' %}
        {% set rgb = '150, 29, 29' %}
      {% elif level_txt == 'medium' %}
        {% set rgb = '150, 109, 29' %}
      {% elif level_txt == 'high' %}
        {% set rgb = '29, 150, 59' %}
      {% else %}
        {% set rgb = '74, 74, 74' %}
      {% endif %}

      --liq-color: rgb({{ rgb }});
      --liq-level: {{ level }}%;
      --wave-speed: {{ '3s' if charging else '8s' }};

      border-radius: 12px;
      position: relative;
      overflow: hidden;
      z-index: 0;
      transition: all 0.5s ease;
    }

    mushroom-shape-icon {
      --icon-size: 68px;
      display: flex;
      margin: -18px 0 10px -18px !important;
      padding-right: 15px;
      z-index: 2;
    }

    ha-card::before {
      content: "";
      position: absolute;
      top: 0; left: 0; bottom: 0;
      z-index: -1;
      width: calc(var(--liq-level) - 60px);
      background: var(--liq-color);
      transition: width 0.5s cubic-bezier(0.25, 0.1, 0.25, 1);
    }

    ha-card::after {
      content: "";
      position: absolute;
      z-index: -1;
      display: block;
      width: 120px;
      height: 120px;
      background: var(--liq-color);
      box-shadow: 0 0 25px rgba({{ rgb }}, 0.5);
      border-radius: 40%;
      left: calc(var(--liq-level) - 120px);
      top: calc(50% - 60px);
      animation: spin-wave var(--wave-speed) linear infinite;
      transition: left 0.5s cubic-bezier(0.25, 0.1, 0.25, 1);
    }

    .mushroom-state-item {
      z-index: 2;
      position: relative;
      text-shadow: 0 1px 3px rgba(0,0,0,0.8);
    }

    ha-state-icon {
      color: white !important;
      filter: drop-shadow(0 2px 4px rgba(0,0,0,0.5));
    }

    @keyframes spin-wave {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }
````
</details>

<details>
<summary><strong>Battery card 3</summary>

```yaml
type: custom:mushroom-entity-card
entity: sensor.anasbox_battery_level
tap_action:
  action: more-info
name: AnasBox Phone
primary_info: state
secondary_info: name
icon_color: white
card_mod:
  style: |
    ha-card {
      /* ====== USER SETTINGS ===== */
      {% set charging = is_state('binary_sensor.anasbox_is_charging', 'on') %}

      /* ======= FONT SETTINGS ======= */
      --card-primary-font-size: 15px !important;
      --card-secondary-font-size: 12px !important;
      --card-primary-font-weight: bold !important;
      /* ========================= */

      {% set level_raw = states(config.entity) | float(0) %}
      {% set level = 0 if level_raw < 0 else (100 if level_raw > 100 else level_raw) %}

      {% if charging %}
        {% set rgb1 = '90, 185, 255' %}
        {% set rgb2 = '140, 240, 255' %}
      {% elif level <= 20 %}
        {% set rgb1 = '255, 85, 95' %}
        {% set rgb2 = '255, 150, 140' %}
      {% elif level <= 60 %}
        {% set rgb1 = '255, 175, 70' %}
        {% set rgb2 = '255, 220, 130' %}
      {% else %}
        {% set rgb1 = '95, 225, 135' %}
        {% set rgb2 = '170, 255, 210' %}
      {% endif %}

      --lvl: {{ level }};
      --c1: rgb({{ rgb1 }});
      --c2: rgb({{ rgb2 }});
      --accent-soft: rgba({{ rgb1 }}, 0.18);
      --accent-med: rgba({{ rgb1 }}, 0.32);
      --track: rgba(255,255,255,0.08);

      --border-speed: {{ '2.2s' if charging else '14s' }};
      --stripe-speed: {{ '1.1s' if charging else '6.5s' }};
      --breath-speed: {{ '1.4s' if charging else '3.8s' }};

      box-shadow:
        0 10px 26px rgba(0,0,0,0.38),
        inset 0 1px 0 rgba(255,255,255,0.04),
        inset 0 -1px 0 rgba(0,0,0,0.25);

      padding-bottom: 28px !important;
      transition: all 0.3s ease;
      overflow: hidden;
      isolation: isolate;
    }

    mushroom-shape-icon {
      --icon-size: 68px;
      --shape-color: rgba({{ rgb1 }}, 0.2) !important;
      display: flex;
      # margin: -18px 0 10px -18px !important;
      padding-right: 10px;
      position: relative;
      z-index: 3 !important;
    }
    mushroom-state-item {
      z-index: 3 !important;
      text-shadow: 0 1px 2px rgba(0,0,0,0.45);
    }

    mushroom-state-item::before {
      content: "";
      position: absolute;
      left: 14px;
      right: 14px;
      bottom: 12px;
      height: 12px;
      border-radius: 999px;
      z-index: 1;
      pointer-events: none;
      background: linear-gradient(180deg, rgba(255,255,255,0.05), rgba(255,255,255,0.00)), var(--track);
      box-shadow: inset 0 0 0 1px rgba(255,255,255,0.05), inset 0 2px 5px rgba(0,0,0,0.35);
    }

    mushroom-state-item::after {
      content: "";
      position: absolute;
      left: 14px;
      bottom: 12px;
      height: 12px;
      border-radius: 999px;
      z-index: 2;
      pointer-events: none;
      width: calc((100% - 28px) * {{ level / 100 }});
      background-image:
        linear-gradient(90deg, var(--c1), var(--c2)),
        repeating-linear-gradient(135deg, rgba(255,255,255,0.18) 0px, rgba(255,255,255,0.18) 7px, transparent 7px, transparent 14px),
        linear-gradient(180deg, rgba(255,255,255,0.14), rgba(255,255,255,0.00) 55%);
      background-size: 100% 100%, 28px 28px, 100% 100%;
      background-position: 0 0, 0 0, 0 0;
      background-repeat: no-repeat;
      box-shadow:
        0 0 10px var(--accent-soft),
        0 0 18px var(--accent-med),
        inset 0 0 0 1px rgba(255,255,255,0.10),
        inset 0 2px 6px rgba(0,0,0,0.25);
      transition: width 0.45s cubic-bezier(0.2, 0.85, 0.2, 1);
      animation:
        stripes-move var(--stripe-speed) linear infinite,
        fill-breathe var(--breath-speed) ease-in-out infinite;
    }

    ha-state-icon {
      opacity: 0.96;
      filter: drop-shadow(0 2px 3px rgba(0,0,0,0.45)) drop-shadow(0 0 8px var(--accent-soft));
    }

    @keyframes stripes-move {
      0% { background-position: 0 0, 0 0, 0 0; }
      100% { background-position: 0 0, 28px 0, 0 0; }
    }

    @keyframes fill-breathe {
      0%, 100% { filter: brightness(1) saturate(1); }
      50% { filter: brightness(1.14) saturate(1.08); }
    }

```
</details>

<details>
<summary><strong>Battery card 3 (low, medium, high)</summary>

```yaml
type: custom:mushroom-entity-card
entity: sensor.anasbox_battery_level
tap_action:
  action: more-info
name: AnasBox Phone
primary_info: state
secondary_info: name
icon_color: white
card_mod:
  style: |
    ha-card {
      /* ====== USER SETTINGS ===== */
      {% set charging = is_state('binary_sensor.anasbox_is_charging', 'on') %}
      
      /* ======= FONT SETTINGS ======= */
      --card-primary-font-size: 15px !important;
      --card-secondary-font-size: 12px !important;
      --card-primary-font-weight: bold !important;
      /* ========================= */

      {% set level_txt = states(config.entity) | lower %}
      {% set level = 20 if level_txt == 'low' else (50 if level_txt == 'medium' else 100) %}

      {% if charging %}
        {% set rgb1 = '90, 185, 255' %}
        {% set rgb2 = '140, 240, 255' %}
      {% elif level_txt == 'low' %}
        {% set rgb1 = '255, 85, 95' %}
        {% set rgb2 = '255, 150, 140' %}
      {% elif level_txt == 'medium' %}
        {% set rgb1 = '255, 175, 70' %}
        {% set rgb2 = '255, 220, 130' %}
      {% elif level_txt == 'high' %}
        {% set rgb1 = '95, 225, 135' %}
        {% set rgb2 = '170, 255, 210' %}
      {% else %}
        {% set rgb1 = '74, 74, 74' %}
        {% set rgb2 = '75, 75, 75' %}
      {% endif %}

      --lvl: {{ level }};
      --c1: rgb({{ rgb1 }});
      --c2: rgb({{ rgb2 }});
      --accent-soft: rgba({{ rgb1 }}, 0.18);
      --accent-med: rgba({{ rgb1 }}, 0.32);
      --track: rgba(255,255,255,0.08);

      --border-speed: {{ '2.2s' if charging else '14s' }};
      --stripe-speed: {{ '1.1s' if charging else '6.5s' }};
      --breath-speed: {{ '1.4s' if charging else '3.8s' }};

      box-shadow:
        0 10px 26px rgba(0,0,0,0.38),
        inset 0 1px 0 rgba(255,255,255,0.04),
        inset 0 -1px 0 rgba(0,0,0,0.25);

      padding-bottom: 28px !important;
      transition: all 0.3s ease;
      overflow: hidden;
      isolation: isolate;
    }

    mushroom-shape-icon {
      --icon-size: 68px;
      --shape-color: rgba({{ rgb1 }}, 0.2) !important;
      display: flex;
      # margin: -18px 0 10px -18px !important;
      padding-right: 10px;
      z-index: 2;
    }

    mushroom-state-item {
      z-index: 2;
      text-shadow: 0 1px 2px rgba(0,0,0,0.45);
    }

    mushroom-state-item::before {
      content: "";
      position: absolute;
      left: 14px;
      right: 14px;
      bottom: 12px;
      height: 12px;
      border-radius: 999px;
      z-index: 0;
      pointer-events: none;
      background: linear-gradient(180deg, rgba(255,255,255,0.05), rgba(255,255,255,0.00)), var(--track);
      box-shadow: inset 0 0 0 1px rgba(255,255,255,0.05), inset 0 2px 5px rgba(0,0,0,0.35);
    }

    mushroom-state-item::after {
      content: "";
      position: absolute;
      left: 14px;
      bottom: 12px;
      height: 12px;
      border-radius: 999px;
      z-index: 1;
      pointer-events: none;
      width: calc((100% - 28px) * {{ level / 100 }});
      background-image:
        linear-gradient(90deg, var(--c1), var(--c2)),
        repeating-linear-gradient(135deg, rgba(255,255,255,0.18) 0px, rgba(255,255,255,0.18) 7px, transparent 7px, transparent 14px),
        linear-gradient(180deg, rgba(255,255,255,0.14), rgba(255,255,255,0.00) 55%);
      background-size: 100% 100%, 28px 28px, 100% 100%;
      background-position: 0 0, 0 0, 0 0;
      background-repeat: no-repeat;
      box-shadow:
        0 0 10px var(--accent-soft),
        0 0 18px var(--accent-med),
        inset 0 0 0 1px rgba(255,255,255,0.10),
        inset 0 2px 6px rgba(0,0,0,0.25);
      transition: width 0.45s cubic-bezier(0.2, 0.85, 0.2, 1);
      animation:
        stripes-move var(--stripe-speed) linear infinite,
        fill-breathe var(--breath-speed) ease-in-out infinite;
    }

    ha-state-icon {
      color: white !important;
      opacity: 0.96;
      filter: drop-shadow(0 2px 3px rgba(0,0,0,0.45)) drop-shadow(0 0 8px var(--accent-soft));
    }

    @keyframes stripes-move {
      0% { background-position: 0 0, 0 0, 0 0; }
      100% { background-position: 0 0, 28px 0, 0 0; }
    }

    @keyframes fill-breathe {
      0%, 100% { filter: brightness(1) saturate(1); }
      50% { filter: brightness(1.14) saturate(1.08); }
    }

````
</details>

---

[paypal_me_shield]: https://img.shields.io/badge/PayPal-00457C?style=for-the-badge&logo=paypal&logoColor=white

[paypal_me]: https://paypal.me/anasboxsupport

[revolut_me_shield]:
https://img.shields.io/badge/revolut-FFFFFF?style=for-the-badge&logo=revolut&logoColor=black

[revolut_me]: https://revolut.me/anas4e

[ko_fi_shield]: https://img.shields.io/badge/Ko--fi-F16061?style=for-the-badge&logo=ko-fi&logoColor=white

[ko_fi_me]: https://ko-fi.com/anasbox

[buy_me_coffee_shield]: 
https://img.shields.io/badge/Buy%20Me%20Coffee-ffdd00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black

[buy_me_coffee_me]: https://www.buymeacoffee.com/anasbox

[patreon_shield]: 
https://img.shields.io/badge/patreon-404040?style=for-the-badge&logo=patreon&logoColor=white

[patreon_me]:  https://patreon.com/AnasBox
