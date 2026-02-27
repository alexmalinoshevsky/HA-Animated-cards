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

# 🏠 Home Assistant Animated Badges and Buttons Maker

> For step-by-step instructions, watch my [YouTube Video](https://youtube.com) here.

<img width="1305" height="530" alt="555616448-8c75d3fa-dfcf-4c36-ac74-f578395d5660" src="https://github.com/user-attachments/assets/ee5965ba-53bf-4037-a379-a09fa6766a6d" />


## $${\color{yellow}For \space Normal \space Entities}$$

<details>
<summary><strong>Custom Button/Badge Card</summary>

```yaml
type: custom:mushroom-chips-card
chips:
  - type: template
    entity: sensor.your_entity_here
    icon: mdi:bell-ring
    content: Button
alignment: end
card_mod:
  style: >
    ha-card {
      /* ================================ */
      /* USER CONFIG HERE                 */
      /* ================================ */

      /* 1. ACTIVE STATES */
      {% set active_states = ['on'] %}
      
      /* 2. POSITIONING */
      {% set pos_y = 'top' %}                
      {% set pos_y_val = '15px' %}
      {% set pos_x = 'right' %}              
      {% set pos_x_val = '15px' %}

      /* 3. SIZING */
      {% set icon_size = '16px' %}          
      {% set text_size = '13px' %}          
      {% set badge_height = '10px' %}        
      {% set border_radius = '5px' %}      
      {% set border_thickness = '0.5px' %}
      
      /* 4. COLORS (HEX, RGB, Color Name) */
      /* --- Active --- */
      {% set active_bg = 'white' %} 
      {% set active_icon = 'white' %}
      {% set active_text = 'white' %}
      {% set active_border = 'white' %}
      {% set active_opacity = 0.3 %}
      
      /* --- Inactive --- */
      {% set inactive_bg = 'grey' %}
      {% set inactive_icon = 'grey' %}
      {% set inactive_text = 'darkgrey' %}
      {% set inactive_border = 'transparent' %}
      {% set inactive_opacity = 0.3 %}
      

      /* 5. ANIMATIONS */
      /* Options: pulse, shake, wobble   */
      /* flash, heartbeat, swing, vibrate  */
      
      {% set anim_type = 'none' %}

      /* true = 24/7, false = only when active */
      {% set always_animate = false %}
      
      {% set anim_intensity_sec = 0.5 %} /* 0.1 to 1  */
      {% set anim_duration_sec = 1 %}
      {% set anim_delay_sec = 1 %}

      /* ================================== */
      /* USER CONFIG END (DONT TOUCH BELOW) */
      /* ================================== */

      {% set current_state = states(config.chips[0].entity) %}
      {% set is_active = current_state in active_states %}
      
      {% if is_active %}
        {% set current_bg = active_bg %}
        {% set current_icon = active_icon %}
        {% set current_text = active_text %}
        {% set current_border = active_border %}
        {% set current_op = active_opacity %}
        {% set current_anim = anim_type %}
      {% else %}
        {% set current_bg = inactive_bg %}
        {% set current_icon = inactive_icon %}
        {% set current_text = inactive_text %}
        {% set current_border = inactive_border %}
        {% set current_op = inactive_opacity %}
        {% set current_anim = anim_type if always_animate else 'none' %}
      {% endif %}

      {% set master_cycle = anim_duration_sec + anim_delay_sec %}
      {% set iterations = (anim_duration_sec / anim_intensity_sec) | round(0, 'floor') | int if anim_intensity_sec > 0 else 0 %}
      {% set step_percent = (anim_intensity_sec / master_cycle) * 100 if master_cycle > 0 else 0 %}

      --chip-padding: {{ badge_height }};
      --chip-border-radius: {{ border_radius }};
      
      --chip-border-width: 0px !important;
      --ha-card-border-width: 0px !important;
      
      --chip-box-shadow: 0 0 0 {{ border_thickness }} {{ current_border }} !important;

      --text-color: {{ current_icon }} !important;
      --color: {{ current_icon }} !important; 
      --chip-background: color-mix(in srgb, {{ current_bg }}, transparent {{ (1 - current_op) * 100 }}%) !important;
      --chip-font-size: {{ text_size }} !important;
      --chip-icon-size: {{ icon_size }} !important; 
      --mdc-icon-size: {{ icon_size }} !important;   
      --chip-height: auto !important;
      --primary-text-color: {{ current_text }} !important;
      --secondary-text-color: {{ current_text }} !important;

      position: absolute !important;
      {{ pos_y }}: {{ pos_y_val }};
      {{ pos_x }}: {{ pos_x_val }};
      z-index: 1;
      border: none !important;
      border-radius: {{ border_radius }} !important;
      box-shadow: 0 4px 6px rgba(0,0,0,0.2);
      overflow: visible !important;
      will-change: transform; 
      
      {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}
        animation: anim-{{ config.chips[0].entity | replace('.', '-') }}-{{ anim_type }} {{ master_cycle }}s infinite linear;
      {% endif %}
    }


    ha-state-icon, ha-icon {
      display: flex;
      align-items: center;
      justify-content: center;
      width: {{ icon_size }} !important; 
      height: {{ icon_size }} !important;
    }


    {% if config.chips[0].content is not defined or config.chips[0].content |
    length == 0 %}
       ha-card { padding: 5px !important; border-radius: 50% !important; aspect-ratio: 1/1; justify-content: center; }
       .content { display: none; }
    {% else %}
       ha-card { padding: 0 10px !important; }
       .content { padding-left: 5px; font-weight: bold; }
    {% endif %}


    {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}

    @keyframes anim-{{ config.chips[0].entity | replace('.', '-') }}-{{
    anim_type }} {
      {% for i in range(iterations) %}
        {% set t = i * step_percent %} 
        
        {% if anim_type == 'pulse' %}
          {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
          {{ t + (step_percent * 0.5) }}% { transform: scale3d(1.15, 1.15, 1); }
          {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

        {% elif anim_type == 'shake' %}
          {{ t + 0 }}% { transform: rotateZ(0deg); }
          {{ t + (step_percent * 0.25) }}% { transform: rotateZ(-10deg); }
          {{ t + (step_percent * 0.75) }}% { transform: rotateZ(10deg); }
          {{ t + step_percent }}% { transform: rotateZ(0deg); }

        {% elif anim_type == 'flash' %}
          {{ t + 0 }}% { opacity: 1; }
          {{ t + (step_percent * 0.5) }}% { opacity: 0; }
          {{ t + step_percent }}% { opacity: 1; }

        {% elif anim_type == 'wobble' %}
          {{ t + 0 }}% { transform: translate3d(0%, 0, 0); }
          {{ t + (step_percent * 0.3) }}% { transform: translate3d(-15%, 0, 0) rotateZ(-5deg); }
          {{ t + (step_percent * 0.6) }}% { transform: translate3d(10%, 0, 0) rotateZ(3deg); }
          {{ t + step_percent }}% { transform: translate3d(0%, 0, 0); }

        {% elif anim_type == 'heartbeat' %}
          {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
          {{ t + (step_percent * 0.25) }}% { transform: scale3d(1.15, 1.15, 1); }
          {{ t + (step_percent * 0.5) }}% { transform: scale3d(1, 1, 1); }
          {{ t + (step_percent * 0.75) }}% { transform: scale3d(1.15, 1.15, 1); }
          {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

        {% elif anim_type == 'swing' %}
          {{ t + 0 }}% { transform: rotateZ(0deg); }
          {{ t + (step_percent * 0.4) }}% { transform: rotateZ(15deg); }
          {{ t + (step_percent * 0.8) }}% { transform: rotateZ(-10deg); }
          {{ t + step_percent }}% { transform: rotateZ(0deg); }

        {% elif anim_type == 'vibrate' %}
          {{ t + 0 }}% { transform: translate3d(0, 0, 0); }
          {{ t + (step_percent * 0.2) }}% { transform: translate3d(-2px, 2px, 0); }
          {{ t + (step_percent * 0.4) }}% { transform: translate3d(-2px, -2px, 0); }
          {{ t + (step_percent * 0.6) }}% { transform: translate3d(2px, 2px, 0); }
          {{ t + (step_percent * 0.8) }}% { transform: translate3d(2px, -2px, 0); }
          {{ t + step_percent }}% { transform: translate3d(0, 0, 0); }

        {% endif %}
      {% endfor %}
    }

    {% endif %}


```
</details>

#### Templates

<details>
<summary><strong>icon & icon color</summary>

```yaml
{% if states(entity) in ['on'] %}

{% else %} 

{% endif %}
```
</details>

<details>
<summary><strong>icon & icon color (multiple states)</summary>

```yaml
{% if states(entity) in ['on', 'off', 'etc'] %}

{% else %} 

{% endif %}
```
</details>

<details>
<summary><strong>content</summary>

```yaml
{{states(entity)}}
```
</details>

## $${\color{yellow}For \space Numeric \space Entities}$$

<details>
<summary><strong>Custom Button/Badge Card</summary>

```yaml
type: custom:mushroom-chips-card
chips:
  - type: template
    entity: sensor.your_(NUMERIC)_entity_here
    icon: mdi:battery-alert
    content: Badge
alignment: end
card_mod:
  style: >
    ha-card {
      /* ================================== */
      /* USER CONFIG HERE                   */
      /* ================================== */

      /* 1. ACTIVE STATE (NUMERIC) */
      {% set threshold_below = 20 %}
      {% set threshold_above = false %}

      /* 2. POSITIONING */
      {% set pos_y = 'top' %}                
      {% set pos_y_val = '10px' %}
      {% set pos_x = 'right' %}              
      {% set pos_x_val = '5px' %}

      /* 3. SIZING */
      {% set icon_size = '18px' %}          
      {% set text_size = '12px' %}          
      {% set badge_height = '5px' %}        
      {% set border_radius = '10px' %}      
      {% set border_thickness = '1px' %}
      
      /* 4. COLORS (HEX, RGB, Color Name) */
      /* --- Active --- */
      {% set active_bg = '#a63a3a' %} 
      {% set active_icon = 'white' %}
      {% set active_text = 'white' %}
      {% set active_border = '#ab1c1c' %}
      {% set active_opacity = 0.3 %}
      
      /* --- Inactive --- */
      {% set inactive_bg = 'grey' %}
      {% set inactive_icon = 'grey' %}
      {% set inactive_text = 'darkgrey' %}
      {% set inactive_border = 'transparent' %}
      {% set inactive_opacity = 0.3 %}

      /* 5. ANIMATIONS */
      /* Options: pulse, shake, wobble   */
      /* flash, heartbeat, swing, vibrate  */
      
      {% set anim_type = 'none' %}

      /* true = 24/7, false = only when active */
      {% set always_animate = false %}
      
      {% set anim_intensity_sec = 0.5 %} /* 0.1 to 1  */
      {% set anim_duration_sec = 2 %}
      {% set anim_delay_sec = 5 %}

      /* ================================== */
      /* USER CONFIG END (DONT TOUCH BELOW) */
      /* ================================== */

      {% set current_state = states(config.chips[0].entity) %}
      {% set state_num = current_state | float(-99999) %}
      
      {% set check_below = threshold_below | string | lower != 'false' %}
      {% set check_above = threshold_above | string | lower != 'false' %}
      
      {% set is_active_below = check_below and state_num != -99999 and state_num <= (threshold_below | float(-99999)) %}
      {% set is_active_above = check_above and state_num != -99999 and state_num >= (threshold_above | float(-99999)) %}
      
      {% set is_active = is_active_below or is_active_above %}
      
      {% if is_active %}
        {% set current_bg = active_bg %}
        {% set current_icon = active_icon %}
        {% set current_text = active_text %}
        {% set current_border = active_border %}
        {% set current_op = active_opacity %}
        {% set current_anim = anim_type %}
      {% else %}
        {% set current_bg = inactive_bg %}
        {% set current_icon = inactive_icon %}
        {% set current_text = inactive_text %}
        {% set current_border = inactive_border %}
        {% set current_op = inactive_opacity %}
        {% set current_anim = anim_type if always_animate else 'none' %}
      {% endif %}

      {% set master_cycle = anim_duration_sec + anim_delay_sec %}
      {% set iterations = (anim_duration_sec / anim_intensity_sec) | round(0, 'floor') | int if anim_intensity_sec > 0 else 0 %}
      {% set step_percent = (anim_intensity_sec / master_cycle) * 100 if master_cycle > 0 else 0 %}

      --chip-padding: {{ badge_height }};
      --chip-border-radius: {{ border_radius }};
      
      --chip-border-width: 0px !important;
      --ha-card-border-width: 0px !important;
      
      --chip-box-shadow: 0 0 0 {{ border_thickness }} {{ current_border }} !important;

      --text-color: {{ current_icon }} !important;
      --color: {{ current_icon }} !important; 
      --chip-background: color-mix(in srgb, {{ current_bg }}, transparent {{ (1 - current_op) * 100 }}%) !important;
      --chip-font-size: {{ text_size }} !important;
      --chip-icon-size: {{ icon_size }} !important; 
      --mdc-icon-size: {{ icon_size }} !important;   
      --chip-height: auto !important;
      --primary-text-color: {{ current_text }} !important;
      --secondary-text-color: {{ current_text }} !important;

      position: absolute !important;
      {{ pos_y }}: {{ pos_y_val }};
      {{ pos_x }}: {{ pos_x_val }};
      z-index: 1;
      border: none !important;
      border-radius: {{ border_radius }} !important;
      box-shadow: 0 4px 6px rgba(0,0,0,0.2);
      overflow: visible !important;
      will-change: transform; 
      
      {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}
        animation: anim-{{ config.chips[0].entity | replace('.', '-') }}-{{ anim_type }} {{ master_cycle }}s infinite linear;
      {% endif %}
    }


    ha-state-icon, ha-icon {
      display: flex;
      align-items: center;
      justify-content: center;
      width: {{ icon_size }} !important; 
      height: {{ icon_size }} !important;
    }


    {% if config.chips[0].content is not defined or config.chips[0].content |
    length == 0 %}
       ha-card { padding: 5px !important; border-radius: 50% !important; aspect-ratio: 1/1; justify-content: center; }
       .content { display: none; }
    {% else %}
       ha-card { padding: 0 10px !important; }
       .content { padding-left: 5px; font-weight: bold; }
    {% endif %}


    {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}

    @keyframes anim-{{ config.chips[0].entity | replace('.', '-') }}-{{
    anim_type }} {
      {% for i in range(iterations) %}
        {% set t = i * step_percent %} 
        
        {% if anim_type == 'pulse' %}
          {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
          {{ t + (step_percent * 0.5) }}% { transform: scale3d(1.15, 1.15, 1); }
          {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

        {% elif anim_type == 'shake' %}
          {{ t + 0 }}% { transform: rotateZ(0deg); }
          {{ t + (step_percent * 0.25) }}% { transform: rotateZ(-10deg); }
          {{ t + (step_percent * 0.75) }}% { transform: rotateZ(10deg); }
          {{ t + step_percent }}% { transform: rotateZ(0deg); }

        {% elif anim_type == 'flash' %}
          {{ t + 0 }}% { opacity: 1; }
          {{ t + (step_percent * 0.5) }}% { opacity: 0; }
          {{ t + step_percent }}% { opacity: 1; }

        {% elif anim_type == 'wobble' %}
          {{ t + 0 }}% { transform: translate3d(0%, 0, 0); }
          {{ t + (step_percent * 0.3) }}% { transform: translate3d(-15%, 0, 0) rotateZ(-5deg); }
          {{ t + (step_percent * 0.6) }}% { transform: translate3d(10%, 0, 0) rotateZ(3deg); }
          {{ t + step_percent }}% { transform: translate3d(0%, 0, 0); }

        {% elif anim_type == 'heartbeat' %}
          {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
          {{ t + (step_percent * 0.25) }}% { transform: scale3d(1.15, 1.15, 1); }
          {{ t + (step_percent * 0.5) }}% { transform: scale3d(1, 1, 1); }
          {{ t + (step_percent * 0.75) }}% { transform: scale3d(1.15, 1.15, 1); }
          {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

        {% elif anim_type == 'swing' %}
          {{ t + 0 }}% { transform: rotateZ(0deg); }
          {{ t + (step_percent * 0.4) }}% { transform: rotateZ(15deg); }
          {{ t + (step_percent * 0.8) }}% { transform: rotateZ(-10deg); }
          {{ t + step_percent }}% { transform: rotateZ(0deg); }

        {% elif anim_type == 'vibrate' %}
          {{ t + 0 }}% { transform: translate3d(0, 0, 0); }
          {{ t + (step_percent * 0.2) }}% { transform: translate3d(-2px, 2px, 0); }
          {{ t + (step_percent * 0.4) }}% { transform: translate3d(-2px, -2px, 0); }
          {{ t + (step_percent * 0.6) }}% { transform: translate3d(2px, 2px, 0); }
          {{ t + (step_percent * 0.8) }}% { transform: translate3d(2px, -2px, 0); }
          {{ t + step_percent }}% { transform: translate3d(0, 0, 0); }

        {% endif %}
      {% endfor %}
    }

    {% endif %}


```
</details>

#### Templates

<details>
<summary><strong>icon & icon color</summary>

```yaml
{% if states(entity)|float <= 20 %}

{% elif states(entity)|float <= 50 %}

{% else %}

{% endif %}
```
</details>

<details>
<summary><strong>content</summary>
  
```yaml
{{states(entity)|float|round(0)}}
```
</details>

## $${\color{yellow}Examples}$$

> [!NOTE]
> Here are example cards with custom buttons and badges. Some of them use advanced templates that you can use to create your own custom card. More examples will be added over time.


<img width="474" height="148" alt="Screenshot 2026-02-27 155522" src="https://github.com/user-attachments/assets/9f2d6005-ae84-4863-9d15-1e067fd595be" />

<details>
<summary><strong>Balcony cards</summary>

```yaml
type: custom:vertical-stack-in-card
cards:
  - type: custom:mushroom-entity-card
    entity: cover.balcony_shades
    primary_info: name
    secondary_info: none
    icon: ""
    tap_action:
      action: more-info
    icon_color: white
    name: Shades
    card_mod:
      style:
        .: |
          ha-card {
            /* --- CONFIGURATION --- */
            {% set icon_size = '65px' %}
            {% set reverse_awning = false %}

            /* Visual Limits */
            {% set limit_min_height = 10 %} 
            {% set limit_max_height = 77 %}

            /* Colors */
            {% set color_sky_day = '#263238' %}
            {% set color_sky_night = '#263238' %}
            {% set color_canvas_1 = '#fa811e' %}
            {% set color_canvas_2 = '#cfd8dc' %}
            {% set color_hem_bar = '#b0bec5' %}

            /* Dimensions */
            {% set window_width = '45%' %}
            {% set card_height = '135px' %}
            
            {% set error_red = 'darkgray' %}
            
            /* --- LOGIC --- */
            {% set status = states(config.entity) %}
            
            {% if status in ['unavailable', 'unknown'] %}
               /* ERROR STATE: Set Red BG and hide awning */
               {% set current_bg = error_red %}
               {% set draw_height = 0 %}
               {% set is_moving = False %}
            {% else %}
               /* NORMAL STATE: Calculate position */
               {% set pos = state_attr(config.entity, 'current_position') | default(0) | int %}
               {% set current_bg = color_sky_night if pos < 10 else color_sky_day %}
               {% set percent_closed = (pos / 100) if reverse_awning else ((100 - pos) / 100) %}
               {% set draw_height = limit_min_height + ((limit_max_height - limit_min_height) * percent_closed) %}
               {% set is_moving = True if status in ['opening', 'closing'] else False %}
            {% endif %}

            /* --- CSS VARS --- */
            --c-window-bg: {{ current_bg }};
            --c-canvas-1: {{ color_canvas_1 }};
            --c-canvas-2: {{ color_canvas_2 }};
            --c-hem-bar: {{ color_hem_bar }};
            --v-icon-size: {{ icon_size }};
            --v-roller-height: {{ draw_height }}%;
            --v-window-width: {{ window_width }};
            --v-card-height: {{ card_height }};
            --icon-animation: {{ 'gentle-pulse 2s infinite ease-in-out' if is_moving else 'none' }};

            /* --- STYLES --- */
            padding-right: calc( {{ window_width }} + 10px ) !important;
            position: relative;
            height: var(--v-card-height) !important;
            transition: height 0.4s;
            overflow: hidden; 
          }

          ha-card::after {
            content: '';
            position: absolute;
            top: 12px; bottom: 12px; right: 12px;
            width: var(--v-window-width);
            border-radius: 4px;
            box-shadow: inset 0 0 5px rgba(0,0,0,0.3);
            background-color: var(--c-window-bg);
            background-image: 
              linear-gradient(to top, var(--c-hem-bar) 0px, var(--c-hem-bar) 6px, rgba(0,0,0,0.2) 6px, transparent 7px),
              repeating-linear-gradient(to right, var(--c-canvas-1) 0px, var(--c-canvas-1) 10px, var(--c-canvas-2) 10px, var(--c-canvas-2) 20px);
            background-size: 100% var(--v-roller-height);
            background-repeat: no-repeat;
            background-position: top center;
            transition: background-size 0.8s cubic-bezier(0.25, 1, 0.5, 1), background-color 1s ease;
            z-index: 1 ;
          }

          ha-card::before {
            content: '';
            position: absolute;
            bottom: 12px; right: 12px;
            height: 20px; 
            width: var(--v-window-width);
            border-bottom-left-radius: 4px;
            border-bottom-right-radius: 4px;
            pointer-events: none;
            z-index: 2;
            background-image:
              linear-gradient(to bottom, #455a64 0px, #455a64 4px, transparent 4px),
              repeating-linear-gradient(to right, transparent 0px, transparent 10px, #607d8b 10px, #607d8b 12px);
          }
        mushroom-state-info$: |
          .container {
            position: absolute !important;
            top: 10px;
            left: calc(var(--v-icon-size) + 5px); 
            width: auto;
          }
        mushroom-shape-icon$: |
          .shape {
            --icon-size: var(--v-icon-size) !important;
            position: absolute !important;
            top: -75px !important;
            left: -18px !important;
            animation: var(--icon-animation) !important;
            z-index: 1;
          }
          @keyframes gentle-pulse {
            0% { transform: scale(1); box-shadow: 0 0 0 0 rgba(var(--rgb-state-cover), 0.3); }
            50% { transform: scale(1.1); box-shadow: 0 0 8px 0 rgba(var(--rgb-state-cover), 0.6); }
            100% { transform: scale(1); box-shadow: 0 0 0 0 rgba(var(--rgb-state-cover), 0.3); }
          }
  - type: custom:mushroom-chips-card
    chips:
      - type: template
        entity: cover.balcony_shades
        icon: |-
          {% if states(entity) in ['closing'] %}
            mdi:arrow-down-bold-circle-outline
          {% else %} 
            mdi:arrow-down-circle 
          {% endif %}
        tap_action:
          action: perform-action
          perform_action: cover.close_cover
          target:
            entity_id: cover.balcony_shades
          data: {}
    alignment: end
    card_mod:
      style: >
        ha-card {
          /* ================================ */
          /* USER CONFIG HERE                 */
          /* ================================ */

          /* 1. ACTIVE STATES */
          {% set active_states = ['closing'] %}
          
          /* 2. POSITIONING */
          {% set pos_y = 'top' %}               
          {% set pos_y_val = '55%' %}
          {% set pos_x = 'right' %}             
          {% set pos_x_val = '84.5%' %}

          /* 3. SIZING */
          {% set icon_size = '30px' %}          
          {% set text_size = '12px' %}          
          {% set badge_height = '7px' %}        
          {% set border_radius = '10px' %}      
          {% set border_thickness = '0.5px' %}
          
          /* 4. COLORS (HEX, RGB, Color Name) */
          /* --- Active --- */
          {% set active_bg = 'lightblue' %} 
          {% set active_icon = 'white' %}
          {% set active_text = 'white' %}
          {% set active_border = 'transparent' %}
          {% set active_opacity = 0.3 %}
          
          /* --- Inactive --- */
          {% set inactive_bg = 'grey' %}
          {% set inactive_icon = 'white' %}
          {% set inactive_text = 'white' %}
          {% set inactive_border = 'transparent' %}
          {% set inactive_opacity = 0.3 %}
          

          /* 5. ANIMATIONS */
          /* Options: pulse, shake, wobble   */
          /* flash, heartbeat, swing, vibrate  */
          
          {% set anim_type = 'none' %}

          /* true = 24/7, false = only when active */
          {% set always_animate = false %}
          
          {% set anim_intensity_sec = 0.9 %} /* 0.1 to 1  */
          {% set anim_duration_sec = 1 %}
          {% set anim_delay_sec = 0 %}

          /* ================================== */
          /* USER CONFIG END (DONT TOUCH BELOW) */
          /* ================================== */

          {% set current_state = states(config.chips[0].entity) %}
          {% set is_active = current_state in active_states %}
          
          {% if is_active %}
            {% set current_bg = active_bg %}
            {% set current_icon = active_icon %}
            {% set current_text = active_text %}
            {% set current_border = active_border %}
            {% set current_op = active_opacity %}
            {% set current_anim = anim_type %}
          {% else %}
            {% set current_bg = inactive_bg %}
            {% set current_icon = inactive_icon %}
            {% set current_text = inactive_text %}
            {% set current_border = inactive_border %}
            {% set current_op = inactive_opacity %}
            {% set current_anim = anim_type if always_animate else 'none' %}
          {% endif %}

          {% set master_cycle = anim_duration_sec + anim_delay_sec %}
          {% set iterations = (anim_duration_sec / anim_intensity_sec) | round(0, 'floor') | int if anim_intensity_sec > 0 else 0 %}
          {% set step_percent = (anim_intensity_sec / master_cycle) * 100 if master_cycle > 0 else 0 %}

          --chip-padding: {{ badge_height }};
          --chip-border-radius: {{ border_radius }};
          
          --chip-border-width: 0px !important;
          --ha-card-border-width: 0px !important;
          
          --chip-box-shadow: 0 0 0 {{ border_thickness }} {{ current_border }} !important;

          --text-color: {{ current_icon }} !important;
          --color: {{ current_icon }} !important; 
          --chip-background: color-mix(in srgb, {{ current_bg }}, transparent {{ (1 - current_op) * 100 }}%) !important;
          --chip-font-size: {{ text_size }} !important;
          --chip-icon-size: {{ icon_size }} !important; 
          --mdc-icon-size: {{ icon_size }} !important;   
          --chip-height: auto !important;
          --primary-text-color: {{ current_text }} !important;
          --secondary-text-color: {{ current_text }} !important;

          position: absolute !important;
          {{ pos_y }}: {{ pos_y_val }};
          {{ pos_x }}: {{ pos_x_val }};
          z-index: 1;
          border: none !important;
          border-radius: {{ border_radius }} !important;
          box-shadow: 0 4px 6px rgba(0,0,0,0.2);
          overflow: visible !important;
          will-change: transform; 
          
          {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}
            animation: anim-{{ config.chips[0].entity | replace('.', '-') }}-{{ anim_type }} {{ master_cycle }}s infinite linear;
          {% endif %}
        }


        ha-state-icon, ha-icon {
          display: flex;
          align-items: center;
          justify-content: center;
          width: {{ icon_size }} !important; 
          height: {{ icon_size }} !important;
        }


        {% if config.chips[0].content is not defined or config.chips[0].content
        | length == 0 %}
           ha-card { padding: 5px !important; border-radius: 50% !important; aspect-ratio: 1/1; justify-content: center; }
           .content { display: none; }
        {% else %}
           ha-card { padding: 0 10px !important; }
           .content { padding-left: 5px; font-weight: bold; }
        {% endif %}


        {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}

        @keyframes anim-{{ config.chips[0].entity | replace('.', '-') }}-{{
        anim_type }} {
          {% for i in range(iterations) %}
            {% set t = i * step_percent %} 
            
            {% if anim_type == 'pulse' %}
              {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.5) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

            {% elif anim_type == 'shake' %}
              {{ t + 0 }}% { transform: rotateZ(0deg); }
              {{ t + (step_percent * 0.25) }}% { transform: rotateZ(-10deg); }
              {{ t + (step_percent * 0.75) }}% { transform: rotateZ(10deg); }
              {{ t + step_percent }}% { transform: rotateZ(0deg); }

            {% elif anim_type == 'flash' %}
              {{ t + 0 }}% { opacity: 1; }
              {{ t + (step_percent * 0.5) }}% { opacity: 0; }
              {{ t + step_percent }}% { opacity: 1; }

            {% elif anim_type == 'wobble' %}
              {{ t + 0 }}% { transform: translate3d(0%, 0, 0); }
              {{ t + (step_percent * 0.3) }}% { transform: translate3d(-15%, 0, 0) rotateZ(-5deg); }
              {{ t + (step_percent * 0.6) }}% { transform: translate3d(10%, 0, 0) rotateZ(3deg); }
              {{ t + step_percent }}% { transform: translate3d(0%, 0, 0); }

            {% elif anim_type == 'heartbeat' %}
              {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.25) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + (step_percent * 0.5) }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.75) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

            {% elif anim_type == 'swing' %}
              {{ t + 0 }}% { transform: rotateZ(0deg); }
              {{ t + (step_percent * 0.4) }}% { transform: rotateZ(15deg); }
              {{ t + (step_percent * 0.8) }}% { transform: rotateZ(-10deg); }
              {{ t + step_percent }}% { transform: rotateZ(0deg); }

            {% elif anim_type == 'vibrate' %}
              {{ t + 0 }}% { transform: translate3d(0, 0, 0); }
              {{ t + (step_percent * 0.2) }}% { transform: translate3d(-2px, 2px, 0); }
              {{ t + (step_percent * 0.4) }}% { transform: translate3d(-2px, -2px, 0); }
              {{ t + (step_percent * 0.6) }}% { transform: translate3d(2px, 2px, 0); }
              {{ t + (step_percent * 0.8) }}% { transform: translate3d(2px, -2px, 0); }
              {{ t + step_percent }}% { transform: translate3d(0, 0, 0); }

            {% endif %}
          {% endfor %}
        }

        {% endif %}
  - type: custom:mushroom-chips-card
    chips:
      - type: template
        entity: cover.balcony_shades
        icon: |-
          {% if states(entity) in ['opening', 'closing'] %}
            mdi:stop-circle-outline
          {% else %} 
            mdi:stop-circle
          {% endif %}
        tap_action:
          action: perform-action
          perform_action: cover.stop_cover
          target:
            entity_id: cover.balcony_shades
          data: {}
    alignment: end
    card_mod:
      style: >
        ha-card {
          /* ================================ */
          /* USER CONFIG HERE                 */
          /* ================================ */

          /* 1. ACTIVE STATES */
          {% set active_states = ['opening', 'closing'] %}
          
          /* 2. POSITIONING */
          {% set pos_y = 'top' %}               
          {% set pos_y_val = '55%' %}
          {% set pos_x = 'right' %}             
          {% set pos_x_val = '69%' %}

          /* 3. SIZING */
          {% set icon_size = '30px' %}          
          {% set text_size = '12px' %}          
          {% set badge_height = '7px' %}        
          {% set border_radius = '10px' %}      
          {% set border_thickness = '0.5px' %}
          
          /* 4. COLORS (HEX, RGB, Color Name) */
          /* --- Active --- */
          {% set active_bg = '#ff5454' %} 
          {% set active_icon = 'white' %}
          {% set active_text = 'white' %}
          {% set active_border = 'transparent' %}
          {% set active_opacity = 0.3 %}
          
          /* --- Inactive --- */
          {% set inactive_bg = 'grey' %}
          {% set inactive_icon = 'white' %}
          {% set inactive_text = 'white' %}
          {% set inactive_border = 'transparent' %}
          {% set inactive_opacity = 0.3 %}
          

          /* 5. ANIMATIONS */
          /* Options: pulse, shake, wobble   */
          /* flash, heartbeat, swing, vibrate  */
          
          {% set anim_type = 'none' %}

          /* true = 24/7, false = only when active */
          {% set always_animate = false %}
          
          {% set anim_intensity_sec = 0.9 %} /* 0.1 to 1  */
          {% set anim_duration_sec = 1 %}
          {% set anim_delay_sec = 0 %}

          /* ================================== */
          /* USER CONFIG END (DONT TOUCH BELOW) */
          /* ================================== */

          {% set current_state = states(config.chips[0].entity) %}
          {% set is_active = current_state in active_states %}
          
          {% if is_active %}
            {% set current_bg = active_bg %}
            {% set current_icon = active_icon %}
            {% set current_text = active_text %}
            {% set current_border = active_border %}
            {% set current_op = active_opacity %}
            {% set current_anim = anim_type %}
          {% else %}
            {% set current_bg = inactive_bg %}
            {% set current_icon = inactive_icon %}
            {% set current_text = inactive_text %}
            {% set current_border = inactive_border %}
            {% set current_op = inactive_opacity %}
            {% set current_anim = anim_type if always_animate else 'none' %}
          {% endif %}

          {% set master_cycle = anim_duration_sec + anim_delay_sec %}
          {% set iterations = (anim_duration_sec / anim_intensity_sec) | round(0, 'floor') | int if anim_intensity_sec > 0 else 0 %}
          {% set step_percent = (anim_intensity_sec / master_cycle) * 100 if master_cycle > 0 else 0 %}

          --chip-padding: {{ badge_height }};
          --chip-border-radius: {{ border_radius }};
          
          --chip-border-width: 0px !important;
          --ha-card-border-width: 0px !important;
          
          --chip-box-shadow: 0 0 0 {{ border_thickness }} {{ current_border }} !important;

          --text-color: {{ current_icon }} !important;
          --color: {{ current_icon }} !important; 
          --chip-background: color-mix(in srgb, {{ current_bg }}, transparent {{ (1 - current_op) * 100 }}%) !important;
          --chip-font-size: {{ text_size }} !important;
          --chip-icon-size: {{ icon_size }} !important; 
          --mdc-icon-size: {{ icon_size }} !important;   
          --chip-height: auto !important;
          --primary-text-color: {{ current_text }} !important;
          --secondary-text-color: {{ current_text }} !important;

          position: absolute !important;
          {{ pos_y }}: {{ pos_y_val }};
          {{ pos_x }}: {{ pos_x_val }};
          z-index: 1;
          border: none !important;
          border-radius: {{ border_radius }} !important;
          box-shadow: 0 4px 6px rgba(0,0,0,0.2);
          overflow: visible !important;
          will-change: transform; 
          
          {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}
            animation: anim-{{ config.chips[0].entity | replace('.', '-') }}-{{ anim_type }} {{ master_cycle }}s infinite linear;
          {% endif %}
        }


        ha-state-icon, ha-icon {
          display: flex;
          align-items: center;
          justify-content: center;
          width: {{ icon_size }} !important; 
          height: {{ icon_size }} !important;
        }


        {% if config.chips[0].content is not defined or config.chips[0].content
        | length == 0 %}
           ha-card { padding: 5px !important; border-radius: 50% !important; aspect-ratio: 1/1; justify-content: center; }
           .content { display: none; }
        {% else %}
           ha-card { padding: 0 10px !important; }
           .content { padding-left: 5px; font-weight: bold; }
        {% endif %}


        {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}

        @keyframes anim-{{ config.chips[0].entity | replace('.', '-') }}-{{
        anim_type }} {
          {% for i in range(iterations) %}
            {% set t = i * step_percent %} 
            
            {% if anim_type == 'pulse' %}
              {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.5) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

            {% elif anim_type == 'shake' %}
              {{ t + 0 }}% { transform: rotateZ(0deg); }
              {{ t + (step_percent * 0.25) }}% { transform: rotateZ(-10deg); }
              {{ t + (step_percent * 0.75) }}% { transform: rotateZ(10deg); }
              {{ t + step_percent }}% { transform: rotateZ(0deg); }

            {% elif anim_type == 'flash' %}
              {{ t + 0 }}% { opacity: 1; }
              {{ t + (step_percent * 0.5) }}% { opacity: 0; }
              {{ t + step_percent }}% { opacity: 1; }

            {% elif anim_type == 'wobble' %}
              {{ t + 0 }}% { transform: translate3d(0%, 0, 0); }
              {{ t + (step_percent * 0.3) }}% { transform: translate3d(-15%, 0, 0) rotateZ(-5deg); }
              {{ t + (step_percent * 0.6) }}% { transform: translate3d(10%, 0, 0) rotateZ(3deg); }
              {{ t + step_percent }}% { transform: translate3d(0%, 0, 0); }

            {% elif anim_type == 'heartbeat' %}
              {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.25) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + (step_percent * 0.5) }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.75) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

            {% elif anim_type == 'swing' %}
              {{ t + 0 }}% { transform: rotateZ(0deg); }
              {{ t + (step_percent * 0.4) }}% { transform: rotateZ(15deg); }
              {{ t + (step_percent * 0.8) }}% { transform: rotateZ(-10deg); }
              {{ t + step_percent }}% { transform: rotateZ(0deg); }

            {% elif anim_type == 'vibrate' %}
              {{ t + 0 }}% { transform: translate3d(0, 0, 0); }
              {{ t + (step_percent * 0.2) }}% { transform: translate3d(-2px, 2px, 0); }
              {{ t + (step_percent * 0.4) }}% { transform: translate3d(-2px, -2px, 0); }
              {{ t + (step_percent * 0.6) }}% { transform: translate3d(2px, 2px, 0); }
              {{ t + (step_percent * 0.8) }}% { transform: translate3d(2px, -2px, 0); }
              {{ t + step_percent }}% { transform: translate3d(0, 0, 0); }

            {% endif %}
          {% endfor %}
        }

        {% endif %}
  - type: custom:mushroom-chips-card
    chips:
      - type: template
        entity: cover.balcony_shades
        icon: |-
          {% if states(entity) in ['opening'] %}
            mdi:arrow-up-bold-circle-outline
          {% else %} 
            mdi:arrow-up-circle 
          {% endif %}
        tap_action:
          action: perform-action
          perform_action: cover.open_cover
          target:
            entity_id: cover.balcony_shades
          data: {}
    alignment: end
    card_mod:
      style: >
        ha-card {
          /* ================================ */
          /* USER CONFIG HERE                 */
          /* ================================ */

          /* 1. ACTIVE STATES */
          {% set active_states = ['opening'] %}
          
          /* 2. POSITIONING */
          {% set pos_y = 'top' %}               
          {% set pos_y_val = '55%' %}
          {% set pos_x = 'right' %}             
          {% set pos_x_val = '54%' %}

          /* 3. SIZING */
          {% set icon_size = '30px' %}          
          {% set text_size = '12px' %}          
          {% set badge_height = '7px' %}        
          {% set border_radius = '10px' %}      
          {% set border_thickness = '0.5px' %}
          
          /* 4. COLORS (HEX, RGB, Color Name) */
          /* --- Active --- */
          {% set active_bg = 'lightblue' %} 
          {% set active_icon = 'white' %}
          {% set active_text = 'white' %}
          {% set active_border = 'transparent' %}
          {% set active_opacity = 0.3 %}
          
          /* --- Inactive --- */
          {% set inactive_bg = 'grey' %}
          {% set inactive_icon = 'white' %}
          {% set inactive_text = 'white' %}
          {% set inactive_border = 'transparent' %}
          {% set inactive_opacity = 0.3 %}
          

          /* 5. ANIMATIONS */
          /* Options: pulse, shake, wobble   */
          /* flash, heartbeat, swing, vibrate  */
          
          {% set anim_type = 'none' %}

          /* true = 24/7, false = only when active */
          {% set always_animate = false %}
          
          {% set anim_intensity_sec = 0.9 %} /* 0.1 to 1  */
          {% set anim_duration_sec = 1 %}
          {% set anim_delay_sec = 0 %}

          /* ================================== */
          /* USER CONFIG END (DONT TOUCH BELOW) */
          /* ================================== */

          {% set current_state = states(config.chips[0].entity) %}
          {% set is_active = current_state in active_states %}
          
          {% if is_active %}
            {% set current_bg = active_bg %}
            {% set current_icon = active_icon %}
            {% set current_text = active_text %}
            {% set current_border = active_border %}
            {% set current_op = active_opacity %}
            {% set current_anim = anim_type %}
          {% else %}
            {% set current_bg = inactive_bg %}
            {% set current_icon = inactive_icon %}
            {% set current_text = inactive_text %}
            {% set current_border = inactive_border %}
            {% set current_op = inactive_opacity %}
            {% set current_anim = anim_type if always_animate else 'none' %}
          {% endif %}

          {% set master_cycle = anim_duration_sec + anim_delay_sec %}
          {% set iterations = (anim_duration_sec / anim_intensity_sec) | round(0, 'floor') | int if anim_intensity_sec > 0 else 0 %}
          {% set step_percent = (anim_intensity_sec / master_cycle) * 100 if master_cycle > 0 else 0 %}

          --chip-padding: {{ badge_height }};
          --chip-border-radius: {{ border_radius }};
          
          --chip-border-width: 0px !important;
          --ha-card-border-width: 0px !important;
          
          --chip-box-shadow: 0 0 0 {{ border_thickness }} {{ current_border }} !important;

          --text-color: {{ current_icon }} !important;
          --color: {{ current_icon }} !important; 
          --chip-background: color-mix(in srgb, {{ current_bg }}, transparent {{ (1 - current_op) * 100 }}%) !important;
          --chip-font-size: {{ text_size }} !important;
          --chip-icon-size: {{ icon_size }} !important; 
          --mdc-icon-size: {{ icon_size }} !important;   
          --chip-height: auto !important;
          --primary-text-color: {{ current_text }} !important;
          --secondary-text-color: {{ current_text }} !important;

          position: absolute !important;
          {{ pos_y }}: {{ pos_y_val }};
          {{ pos_x }}: {{ pos_x_val }};
          z-index: 1;
          border: none !important;
          border-radius: {{ border_radius }} !important;
          box-shadow: 0 4px 6px rgba(0,0,0,0.2);
          overflow: visible !important;
          will-change: transform; 
          
          {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}
            animation: anim-{{ config.chips[0].entity | replace('.', '-') }}-{{ anim_type }} {{ master_cycle }}s infinite linear;
          {% endif %}
        }


        ha-state-icon, ha-icon {
          display: flex;
          align-items: center;
          justify-content: center;
          width: {{ icon_size }} !important; 
          height: {{ icon_size }} !important;
        }


        {% if config.chips[0].content is not defined or config.chips[0].content
        | length == 0 %}
           ha-card { padding: 5px !important; border-radius: 50% !important; aspect-ratio: 1/1; justify-content: center; }
           .content { display: none; }
        {% else %}
           ha-card { padding: 0 10px !important; }
           .content { padding-left: 5px; font-weight: bold; }
        {% endif %}


        {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}

        @keyframes anim-{{ config.chips[0].entity | replace('.', '-') }}-{{
        anim_type }} {
          {% for i in range(iterations) %}
            {% set t = i * step_percent %} 
            
            {% if anim_type == 'pulse' %}
              {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.5) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

            {% elif anim_type == 'shake' %}
              {{ t + 0 }}% { transform: rotateZ(0deg); }
              {{ t + (step_percent * 0.25) }}% { transform: rotateZ(-10deg); }
              {{ t + (step_percent * 0.75) }}% { transform: rotateZ(10deg); }
              {{ t + step_percent }}% { transform: rotateZ(0deg); }

            {% elif anim_type == 'flash' %}
              {{ t + 0 }}% { opacity: 1; }
              {{ t + (step_percent * 0.5) }}% { opacity: 0; }
              {{ t + step_percent }}% { opacity: 1; }

            {% elif anim_type == 'wobble' %}
              {{ t + 0 }}% { transform: translate3d(0%, 0, 0); }
              {{ t + (step_percent * 0.3) }}% { transform: translate3d(-15%, 0, 0) rotateZ(-5deg); }
              {{ t + (step_percent * 0.6) }}% { transform: translate3d(10%, 0, 0) rotateZ(3deg); }
              {{ t + step_percent }}% { transform: translate3d(0%, 0, 0); }

            {% elif anim_type == 'heartbeat' %}
              {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.25) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + (step_percent * 0.5) }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.75) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

            {% elif anim_type == 'swing' %}
              {{ t + 0 }}% { transform: rotateZ(0deg); }
              {{ t + (step_percent * 0.4) }}% { transform: rotateZ(15deg); }
              {{ t + (step_percent * 0.8) }}% { transform: rotateZ(-10deg); }
              {{ t + step_percent }}% { transform: rotateZ(0deg); }

            {% elif anim_type == 'vibrate' %}
              {{ t + 0 }}% { transform: translate3d(0, 0, 0); }
              {{ t + (step_percent * 0.2) }}% { transform: translate3d(-2px, 2px, 0); }
              {{ t + (step_percent * 0.4) }}% { transform: translate3d(-2px, -2px, 0); }
              {{ t + (step_percent * 0.6) }}% { transform: translate3d(2px, 2px, 0); }
              {{ t + (step_percent * 0.8) }}% { transform: translate3d(2px, -2px, 0); }
              {{ t + step_percent }}% { transform: translate3d(0, 0, 0); }

            {% endif %}
          {% endfor %}
        }

        {% endif %}

```
</details>

<img width="505" height="118" alt="Screenshot 2026-02-27 155901" src="https://github.com/user-attachments/assets/18a6cacc-073c-46f5-bd2d-39b37ed3c0e0" />

<details>
<summary><strong>TEMP & Humidity</summary>

```yaml
type: horizontal-stack
cards:
  - type: custom:vertical-stack-in-card
    cards:
      - type: custom:mushroom-entity-card
        entity: sensor.livingroom_temperature
        tap_action:
          action: more-info
        icon: mdi:thermometer
        name: Temperature
        primary_info: state
        secondary_info: name
        card_mod:
          style:
            mushroom-shape-icon$: |
              .shape {
                {# ========== CONFIG ========== #}
                {% set temp = states('sensor.livingroom_temperature') | float(0) %}

                {# ------------------------------------------- #}
                {# TEMPERATURE → COLOR + EFFECT PRESETS        #}
                {# ------------------------------------------- #}

                {# DEFAULTS (will be overridden by ranges) #}
                {% set rgb = '0,140,255' %}
                {% set anim = 'temp-cold-breathe' %}
                {% set glow_anim = 'temp-cold-glow' %}
                {% set halo_anim = 'temp-cold-halo' %}
                {% set duration = 4.0 %}
                {% set intensity = 0.5 %}

                {# RANGES / COLORS #}
                {# You can change temp numbers below if needed #}

                {% if temp < 16 %}
                  {# BLUE #}
                  {% set rgb = '0,140,255' %}
                  {% set anim = 'temp-cold-breathe' %}
                  {% set glow_anim = 'temp-cold-glow' %}
                  {% set halo_anim = 'temp-cold-halo' %}
                  {% set duration = 4.4 %}
                  {% set intensity = 0.4 %}

                {% elif temp < 18 %}
                  {# YELLOW #}
                  {% set rgb = '255,210,40' %}
                  {% set anim = 'temp-cool-wave' %}
                  {% set glow_anim = 'temp-cool-glow' %}
                  {% set halo_anim = 'temp-cool-halo' %}
                  {% set duration = 3.4 %}
                  {% set intensity = 0.55 %}

                {% elif temp < 20 %}
                  {# ORANGE #}
                  {% set rgb = '255,150,40' %}
                  {% set anim = 'temp-comfy-breathe' %}
                  {% set glow_anim = 'temp-comfy-glow' %}
                  {% set halo_anim = 'temp-comfy-halo' %}
                  {% set duration = 3.0 %}
                  {% set intensity = 0.6 %}

                {% elif temp < 22 %}
                  {# DARK ORANGE #}
                  {% set rgb = '255,115,20' %}
                  {% set anim = 'temp-warm-pulse' %}
                  {% set glow_anim = 'temp-warm-glow' %}
                  {% set halo_anim = 'temp-warm-halo' %}
                  {% set duration = 2.4 %}
                  {% set intensity = 0.8 %}

                {% else %}
                  {# RED #}
                  {% set rgb = '255,40,40' %}
                  {% set anim = 'temp-hot-shimmer' %}
                  {% set glow_anim = 'temp-hot-glow' %}
                  {% set halo_anim = 'temp-hot-halo' %}
                  {% set duration = 2.0 %}
                  {% set intensity = 1.0 %}
                {% endif %}

                {# Apply variables #}
                --temp-rgb: {{ rgb }};
                --temp-intensity: {{ intensity }};
                --shape-animation: {{ anim }} {{ duration }}s ease-in-out infinite;
                --temp-glow-animation: {{ glow_anim }} {{ (duration * 0.9) | round(2) }}s ease-in-out infinite;
                --temp-halo-animation: {{ halo_anim }} {{ (duration * 1.15) | round(2) }}s ease-in-out infinite;

                opacity: 1;

                /* Icon color follows the temperature */
                --icon-color: rgba({{ rgb }}, 1);

                /* 🔧 KILL THE THEME BLUE & MAKE SHAPE NEUTRAL */
                background-color: rgba(77, 77, 77,0.1) !important;
                box-shadow: none !important;
                border: 1px solid rgba(255,255,255,0.06);

                position: relative;
                transform-origin: 50% 60%;
                animation: var(--shape-animation);
              }

              /* Glow layers */
              .shape::before,
              .shape::after {
                content: '';
                position: absolute;
                border-radius: inherit;
                pointer-events: none;
              }

              .shape::before {
                inset: -8px;
                animation: var(--temp-glow-animation);
              }

              .shape::after {
                inset: -22px;
                animation: var(--temp-halo-animation);
                mix-blend-mode: screen;
              }

              /* ========== COLD ========== */
              @keyframes temp-cold-breathe {
                0%   { transform: scale(0.96); }
                50%  { transform: scale(1.03); }
                100% { transform: scale(0.96); }
              }

              @keyframes temp-cold-glow {
                0% {
                  box-shadow:
                    0 0 20px 0 rgba(var(--temp-rgb), 0.6),
                    0 0 34px 6 rgba(var(--temp-rgb), 0.55);
                }
                50% {
                  box-shadow:
                    0 0 30px 4 rgba(var(--temp-rgb), 0.95),
                    0 0 50px 10px rgba(var(--temp-rgb), 0.85);
                }
                100% {
                  box-shadow:
                    0 0 20px 0 rgba(var(--temp-rgb), 0.6),
                    0 0 34px 6 rgba(var(--temp-rgb), 0.55);
                }
              }

              @keyframes temp-cold-halo {
                0% {
                  box-shadow:
                    0 0 80px 20px rgba(var(--temp-rgb), 0.35),
                    0 -20px 80px -14px rgba(220, 240, 255, 0.55);
                }
                50% {
                  box-shadow:
                    0 0 130px 36px rgba(var(--temp-rgb), 0.5),
                    0 -34px 100px -8px rgba(240, 250, 255, 0.8);
                }
                100% {
                  box-shadow:
                    0 0 80px 20px rgba(var(--temp-rgb), 0.35),
                    0 -20px 80px -14px rgba(220, 240, 255, 0.55);
                }
              }

              /* ========== COOL ========== */
              @keyframes temp-cool-wave {
                0%   { transform: translateX(0); }
                25%  { transform: translateX(-1px); }
                50%  { transform: translateX(1px) translateY(-1px); }
                75%  { transform: translateX(-1px); }
                100% { transform: translateX(0); }
              }

              @keyframes temp-cool-glow {
                0% {
                  box-shadow:
                    0 0 22px 0 rgba(var(--temp-rgb), 0.6),
                    0 0 34px 4 rgba(var(--temp-rgb), 0.7);
                }
                50% {
                  box-shadow:
                    0 0 28px 2 rgba(var(--temp-rgb), 0.95),
                    0 0 48px 12px rgba(var(--temp-rgb), 0.85);
                }
                100% {
                  box-shadow:
                    0 0 22px 0 rgba(var(--temp-rgb), 0.6),
                    0 0 34px 4 rgba(var(--temp-rgb), 0.7);
                }
              }

              @keyframes temp-cool-halo {
                0% {
                  box-shadow:
                    0 0 90px 26px rgba(var(--temp-rgb), 0.35),
                    0 18px 80px -12px rgba(0, 220, 255, 0.35);
                }
                50% {
                  box-shadow:
                    0 0 140px 42px rgba(var(--temp-rgb), 0.45),
                    0 30px 110px -10px rgba(0, 255, 255, 0.5);
                }
                100% {
                  box-shadow:
                    0 0 90px 26px rgba(var(--temp-rgb), 0.35),
                    0 18px 80px -12px rgba(0, 220, 255, 0.35);
                }
              }

              /* ========== COMFY ========== */
              @keyframes temp-comfy-breathe {
                0%   { transform: scale(0.98); }
                50%  { transform: scale(1.05); }
                100% { transform: scale(0.98); }
              }

              @keyframes temp-comfy-glow {
                50% {
                  box-shadow:
                    0 0 26px 4 rgba(var(--temp-rgb), 0.9),
                    0 0 42px 10px rgba(var(--temp-rgb), 0.85);
                }
              }

              @keyframes temp-comfy-halo {
                50% {
                  box-shadow:
                    0 0 120px 40px rgba(var(--temp-rgb), 0.45),
                    0 26px 80px -10px rgba(180,255,200,0.5);
                }
              }

              /* ========== WARM ========== */
              @keyframes temp-warm-pulse {
                0%   { transform: scale(1); }
                50%  { transform: scale(1.07); }
                100% { transform: scale(1); }
              }

              @keyframes temp-warm-glow {
                50% {
                  box-shadow:
                    0 0 30px 4 rgba(var(--temp-rgb), 0.95),
                    0 0 54px 14px rgba(var(--temp-rgb), 0.9);
                }
              }

              @keyframes temp-warm-halo {
                50% {
                  box-shadow:
                    0 0 140px 48px rgba(var(--temp-rgb), 0.55),
                    0 26px 100px -10px rgba(255,210,150,0.5);
                }
              }

              /* ========== HOT ========== */
              @keyframes temp-hot-shimmer {
                0%   { transform: scale(1); filter: blur(0); }
                50%  { transform: scale(1.08); filter: blur(0.6px); }
                100% { transform: scale(1); filter: blur(0); }
              }

              @keyframes temp-hot-glow {
                50% {
                  box-shadow:
                    0 0 34px 6 rgba(var(--temp-rgb), 1),
                    0 0 62px 14px rgba(var(--temp-rgb), 0.95);
                }
              }

              @keyframes temp-hot-halo {
                50% {
                  box-shadow:
                    0 0 160px 60px rgba(var(--temp-rgb), 0.6),
                    0 34px 120px -12px rgba(255,150,100,0.6);
                }
              }
            .: |
              mushroom-shape-icon {
                --icon-size: 64px;
                --icon-color: rgba(var(--hum-rgb),1) !important;
                display: flex;
                margin: -18px 0 10px -20px !important;
                padding-right: 22px;
                padding-bottom: 25px;
              }
              ha-card {
                clip-path: inset(0 0 0 0 round var(--ha-card-border-radius, 14px));
                
                /* FONT SIZE & SPACING SETTINGS */
                --card-primary-font-size: 1.3rem !important;
                
                /* Increases vertical space between primary and secondary */
                --card-primary-line-height: 1.3 !important;
              }
      - type: custom:vertical-stack-in-card
        cards:
          - type: custom:mini-graph-card
            entities:
              - sensor.livingroom_temperature
            hours_to_show: 24
            line_width: 5
            show:
              name: false
              icon: false
              state: false
              labels: false
              legend: false
            color_thresholds:
              - value: 0
                color: blue
              - value: 18
                color: lightblue
              - value: 20
                color: orange
              - value: 22
                color: red
            card_mod:
              style: |
                ha-card {
                  background: none;
                  box-shadow: none;
                  opacity: 50%;
                  border: none;
                  width: 250px;
                  mask-image: radial-gradient(
                    ellipse at center,
                    rgba(0,0,0,1) 0%,
                    rgba(0,0,0,0) 90%
                  );
                }
        card_mod:
          style:
            .: |
              ha-card {
                  background: none;
                  box-shadow: none;
                  border: none;
                  margin: 8px 12px; 
                  position: absolute;
                  bottom: -10px;
                  right: -10px;
                  }
      - type: custom:mushroom-chips-card
        chips:
          - type: template
            entity: sensor.livingroom_mi_battery
            icon: |-
              {% if states(entity)|float<=20 %}
                mdi:battery-20
              {% elif states(entity)|float<=50 %} 
                mdi:battery-50
              {% else %} 
                mdi:battery
              {% endif %}
            content: "{{states(entity)|float|round(0)}}%"
            icon_color: |-
              {% if states(entity)|float<=25 %}
                red
              {% elif states(entity)|float<=75 %} 
                orange
              {% else %} 
                green
              {% endif %}
        alignment: end
        card_mod:
          style: >
            ha-card {
              /* ================================== */
              /* USER CONFIG HERE                   */
              /* ================================== */

              /* 1. ACTIVE STATE (NUMERIC) */
              {% set threshold_below = 20 %}
              {% set threshold_above = false %}

              /* 2. POSITIONING */
              {% set pos_y = 'top' %}               
              {% set pos_y_val = '10px' %}
              {% set pos_x = 'right' %}             
              {% set pos_x_val = '0%' %}

              /* 3. SIZING */
              {% set icon_size = '10px' %}          
              {% set text_size = '8px' %}          
              {% set badge_height = '3px' %}       
              {% set border_radius = '7px' %}       
              {% set border_thickness = '1px' %}
              
              /* 4. COLORS (HEX, RGB, Color Name) */
              /* --- Active --- */
              {% set active_bg = 'grey' %} 
              {% set active_icon = 'white' %}
              {% set active_text = 'darkgrey' %}
              {% set active_border = 'transparent' %}
              {% set active_opacity = 0.3 %}
              
              /* --- Inactive --- */
              {% set inactive_bg = 'grey' %}
              {% set inactive_icon = 'red' %}
              {% set inactive_text = 'white' %}
              {% set inactive_border = 'transparent' %}
              {% set inactive_opacity = 0.3 %}

              /* 5. ANIMATIONS */
              /* Options: pulse, shake, wobble   */
              /* flash, heartbeat, swing, vibrate  */
              
              {% set anim_type = 'flash' %}

              /* true = 24/7, false = only when active */
              {% set always_animate = false %}
              
              {% set anim_intensity_sec = 0.5 %} /* 0.1 to 1  */
              {% set anim_duration_sec = 2 %}
              {% set anim_delay_sec = 5 %}

              /* ================================== */
              /* USER CONFIG END (DONT TOUCH BELOW) */
              /* ================================== */

              {% set current_state = states(config.chips[0].entity) %}
              {% set state_num = current_state | float(-99999) %}
              
              {% set check_below = threshold_below | string | lower != 'false' %}
              {% set check_above = threshold_above | string | lower != 'false' %}
              
              {% set is_active_below = check_below and state_num != -99999 and state_num <= (threshold_below | float(-99999)) %}
              {% set is_active_above = check_above and state_num != -99999 and state_num >= (threshold_above | float(-99999)) %}
              
              {% set is_active = is_active_below or is_active_above %}
              
              {% if is_active %}
                {% set current_bg = active_bg %}
                {% set current_icon = active_icon %}
                {% set current_text = active_text %}
                {% set current_border = active_border %}
                {% set current_op = active_opacity %}
                {% set current_anim = anim_type %}
              {% else %}
                {% set current_bg = inactive_bg %}
                {% set current_icon = inactive_icon %}
                {% set current_text = inactive_text %}
                {% set current_border = inactive_border %}
                {% set current_op = inactive_opacity %}
                {% set current_anim = anim_type if always_animate else 'none' %}
              {% endif %}

              {% set master_cycle = anim_duration_sec + anim_delay_sec %}
              {% set iterations = (anim_duration_sec / anim_intensity_sec) | round(0, 'floor') | int if anim_intensity_sec > 0 else 0 %}
              {% set step_percent = (anim_intensity_sec / master_cycle) * 100 if master_cycle > 0 else 0 %}

              --chip-padding: {{ badge_height }};
              --chip-border-radius: {{ border_radius }};
              
              --chip-border-width: 0px !important;
              --ha-card-border-width: 0px !important;
              
              --chip-box-shadow: 0 0 0 {{ border_thickness }} {{ current_border }} !important;

              --text-color: {{ current_icon }} !important;
              --color: {{ current_icon }} !important; 
              --chip-background: color-mix(in srgb, {{ current_bg }}, transparent {{ (1 - current_op) * 100 }}%) !important;
              --chip-font-size: {{ text_size }} !important;
              --chip-icon-size: {{ icon_size }} !important; 
              --mdc-icon-size: {{ icon_size }} !important;   
              --chip-height: auto !important;
              --primary-text-color: {{ current_text }} !important;
              --secondary-text-color: {{ current_text }} !important;

              position: absolute !important;
              {{ pos_y }}: {{ pos_y_val }};
              {{ pos_x }}: {{ pos_x_val }};
              z-index: 1;
              border: none !important;
              border-radius: {{ border_radius }} !important;
              box-shadow: 0 4px 6px rgba(0,0,0,0.2);
              overflow: visible !important;
              will-change: transform; 
              
              {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}
                animation: anim-{{ config.chips[0].entity | replace('.', '-') }}-{{ anim_type }} {{ master_cycle }}s infinite linear;
              {% endif %}
            }


            ha-state-icon, ha-icon {
              display: flex;
              align-items: center;
              justify-content: center;
              width: {{ icon_size }} !important; 
              height: {{ icon_size }} !important;
            }


            {% if config.chips[0].content is not defined or
            config.chips[0].content | length == 0 %}
               ha-card { padding: 5px !important; border-radius: 50% !important; aspect-ratio: 1/1; justify-content: center; }
               .content { display: none; }
            {% else %}
               ha-card { padding: 0 10px !important; }
               .content { padding-left: 5px; font-weight: bold; }
            {% endif %}


            {% if current_anim != 'none' and iterations > 0 and master_cycle > 0
            %}

            @keyframes anim-{{ config.chips[0].entity | replace('.', '-') }}-{{
            anim_type }} {
              {% for i in range(iterations) %}
                {% set t = i * step_percent %} 
                
                {% if anim_type == 'pulse' %}
                  {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
                  {{ t + (step_percent * 0.5) }}% { transform: scale3d(1.15, 1.15, 1); }
                  {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

                {% elif anim_type == 'shake' %}
                  {{ t + 0 }}% { transform: rotateZ(0deg); }
                  {{ t + (step_percent * 0.25) }}% { transform: rotateZ(-10deg); }
                  {{ t + (step_percent * 0.75) }}% { transform: rotateZ(10deg); }
                  {{ t + step_percent }}% { transform: rotateZ(0deg); }

                {% elif anim_type == 'flash' %}
                  {{ t + 0 }}% { opacity: 1; }
                  {{ t + (step_percent * 0.5) }}% { opacity: 0; }
                  {{ t + step_percent }}% { opacity: 1; }

                {% elif anim_type == 'wobble' %}
                  {{ t + 0 }}% { transform: translate3d(0%, 0, 0); }
                  {{ t + (step_percent * 0.3) }}% { transform: translate3d(-15%, 0, 0) rotateZ(-5deg); }
                  {{ t + (step_percent * 0.6) }}% { transform: translate3d(10%, 0, 0) rotateZ(3deg); }
                  {{ t + step_percent }}% { transform: translate3d(0%, 0, 0); }

                {% elif anim_type == 'heartbeat' %}
                  {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
                  {{ t + (step_percent * 0.25) }}% { transform: scale3d(1.15, 1.15, 1); }
                  {{ t + (step_percent * 0.5) }}% { transform: scale3d(1, 1, 1); }
                  {{ t + (step_percent * 0.75) }}% { transform: scale3d(1.15, 1.15, 1); }
                  {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

                {% elif anim_type == 'swing' %}
                  {{ t + 0 }}% { transform: rotateZ(0deg); }
                  {{ t + (step_percent * 0.4) }}% { transform: rotateZ(15deg); }
                  {{ t + (step_percent * 0.8) }}% { transform: rotateZ(-10deg); }
                  {{ t + step_percent }}% { transform: rotateZ(0deg); }

                {% elif anim_type == 'vibrate' %}
                  {{ t + 0 }}% { transform: translate3d(0, 0, 0); }
                  {{ t + (step_percent * 0.2) }}% { transform: translate3d(-2px, 2px, 0); }
                  {{ t + (step_percent * 0.4) }}% { transform: translate3d(-2px, -2px, 0); }
                  {{ t + (step_percent * 0.6) }}% { transform: translate3d(2px, 2px, 0); }
                  {{ t + (step_percent * 0.8) }}% { transform: translate3d(2px, -2px, 0); }
                  {{ t + step_percent }}% { transform: translate3d(0, 0, 0); }

                {% endif %}
              {% endfor %}
            }

            {% endif %}
  - type: custom:vertical-stack-in-card
    cards:
      - type: custom:mushroom-entity-card
        entity: sensor.livingroom_humidity
        tap_action:
          action: more-info
        icon: mdi:water-percent
        name: Humidity
        primary_info: state
        secondary_info: name
        card_mod:
          style:
            mushroom-shape-icon$: |
              .shape {
                {# ========== CONFIG ========== #}
                {% set hum = states('sensor.livingroom_humidity') | float(0) %}

                {# DEFAULTS #}
                {% set rgb = '120,210,255' %}
                {% set anim = 'hum-good-breathe' %}
                {% set glow_anim = 'hum-good-glow' %}
                {% set halo_anim = 'hum-good-halo' %}
                {% set duration = 3.2 %}

                {# RANGES
                   < 40   -> BAD (dark blue)
                   40-60  -> GOOD (light blue)
                   > 60   -> MIDDLE / humid (medium blue)
                #}

                {% if hum < 40 %}
                  {# BAD - dry - dark blue #}
                  {% set rgb = '0,80,200' %}
                  {% set anim = 'hum-bad-pulse' %}
                  {% set glow_anim = 'hum-bad-glow' %}
                  {% set halo_anim = 'hum-bad-halo' %}
                  {% set duration = 2.8 %}
                {% elif hum <= 60 %}
                  {# GOOD - light blue #}
                  {% set rgb = '120,210,255' %}
                  {% set anim = 'hum-good-breathe' %}
                  {% set glow_anim = 'hum-good-glow' %}
                  {% set halo_anim = 'hum-good-halo' %}
                  {% set duration = 3.4 %}
                {% else %}
                  {# MIDDLE / humid - medium blue #}
                  {% set rgb = '40,140,255' %}
                  {% set anim = 'hum-mid-wave' %}
                  {% set glow_anim = 'hum-mid-glow' %}
                  {% set halo_anim = 'hum-mid-halo' %}
                  {% set duration = 3.0 %}
                {% endif %}

                --hum-rgb: {{ rgb }};
                --shape-animation: {{ anim }} {{ duration }}s ease-in-out infinite;
                --hum-glow-animation: {{ glow_anim }} {{ (duration * 0.9) | round(2) }}s ease-in-out infinite;
                --hum-halo-animation: {{ halo_anim }} {{ (duration * 1.1) | round(2) }}s ease-in-out infinite;

                /* Icon color follows humidity color */
                --icon-color: rgba({{ rgb }}, 1);

                /* Neutral pill so theme blue does not leak through */
                background-color: rgba(77,77,77,0.2) !important;
                box-shadow: none !important;
                border: 1px solid rgba(255,255,255,0.06);

                opacity: 1;
                position: relative;
                transform-origin: 50% 60%;
                animation: var(--shape-animation);
              }

              /* Glow layers */
              .shape::before,
              .shape::after {
                content: '';
                position: absolute;
                border-radius: inherit;
                pointer-events: none;
              }

              .shape::before {
                inset: -8px;
                animation: var(--hum-glow-animation);
              }

              .shape::after {
                inset: -22px;
                animation: var(--hum-halo-animation);
                mix-blend-mode: screen;
              }

              /* ========== GOOD - light blue ========== */
              @keyframes hum-good-breathe {
                0%   { transform: scale(0.98); }
                50%  { transform: scale(1.04); }
                100% { transform: scale(0.98); }
              }

              @keyframes hum-good-glow {
                0% {
                  box-shadow:
                    0 0 18px 0 rgba(var(--hum-rgb), 0.55),
                    0 0 30px 4 rgba(var(--hum-rgb), 0.6);
                }
                50% {
                  box-shadow:
                    0 0 24px 4 rgba(var(--hum-rgb), 0.9),
                    0 0 44px 10px rgba(var(--hum-rgb), 0.85);
                }
                100% {
                  box-shadow:
                    0 0 18px 0 rgba(var(--hum-rgb), 0.55),
                    0 0 30px 4 rgba(var(--hum-rgb), 0.6);
                }
              }

              @keyframes hum-good-halo {
                0% {
                  box-shadow:
                    0 0 80px 24px rgba(var(--hum-rgb), 0.35),
                    0 18px 70px -12px rgba(180,230,255,0.3);
                }
                50% {
                  box-shadow:
                    0 0 120px 40px rgba(var(--hum-rgb), 0.45),
                    0 26px 90px -10px rgba(200,240,255,0.45);
                }
                100% {
                  box-shadow:
                    0 0 80px 24px rgba(var(--hum-rgb), 0.35),
                    0 18px 70px -12px rgba(180,230,255,0.3);
                }
              }

              /* ========== MIDDLE / humid - medium blue ========== */
              @keyframes hum-mid-wave {
                0%   { transform: translateX(0); }
                25%  { transform: translateX(-1px); }
                50%  { transform: translateX(1px) translateY(-1px); }
                75%  { transform: translateX(-1px); }
                100% { transform: translateX(0); }
              }

              @keyframes hum-mid-glow {
                0% {
                  box-shadow:
                    0 0 20px 0 rgba(var(--hum-rgb), 0.6),
                    0 0 32px 4 rgba(var(--hum-rgb), 0.7);
                }
                50% {
                  box-shadow:
                    0 0 28px 3 rgba(var(--hum-rgb), 0.95),
                    0 0 48px 10px rgba(var(--hum-rgb), 0.85);
                }
                100% {
                  box-shadow:
                    0 0 20px 0 rgba(var(--hum-rgb), 0.6),
                    0 0 32px 4 rgba(var(--hum-rgb), 0.7);
                }
              }

              @keyframes hum-mid-halo {
                0% {
                  box-shadow:
                    0 0 90px 26px rgba(var(--hum-rgb), 0.4),
                    0 18px 80px -12px rgba(80,190,255,0.35);
                }
                50% {
                  box-shadow:
                    0 0 135px 42px rgba(var(--hum-rgb), 0.5),
                    0 28px 105px -10px rgba(100,210,255,0.5);
                }
                100% {
                  box-shadow:
                    0 0 90px 26px rgba(var(--hum-rgb), 0.4),
                    0 18px 80px -12px rgba(80,190,255,0.35);
                }
              }

              /* ========== BAD - dry - dark blue ========== */
              @keyframes hum-bad-pulse {
                0%   { transform: scale(0.97); }
                40%  { transform: scale(1.03); }
                100% { transform: scale(0.97); }
              }

              @keyframes hum-bad-glow {
                0% {
                  box-shadow:
                    0 0 18px 0 rgba(var(--hum-rgb), 0.6),
                    0 0 28px 4 rgba(var(--hum-rgb), 0.7);
                }
                50% {
                  box-shadow:
                    0 0 26px 4 rgba(var(--hum-rgb), 0.95),
                    0 0 44px 12px rgba(var(--hum-rgb), 0.9);
                }
                100% {
                  box-shadow:
                    0 0 18px 0 rgba(var(--hum-rgb), 0.6),
                    0 0 28px 4 rgba(var(--hum-rgb), 0.7);
                }
              }

              @keyframes hum-bad-halo {
                0% {
                  box-shadow:
                    0 0 80px 22px rgba(var(--hum-rgb), 0.45),
                    0 18px 75px -10px rgba(0,70,160,0.5);
                }
                50% {
                  box-shadow:
                    0 0 130px 40px rgba(var(--hum-rgb), 0.6),
                    0 26px 100px -8px rgba(0,90,190,0.65);
                }
                100% {
                  box-shadow:
                    0 0 80px 22px rgba(var(--hum-rgb), 0.45),
                    0 18px 75px -10px rgba(0,70,160,0.5);
                }
              }
            .: |
              mushroom-shape-icon {
                --icon-size: 64px;
                --icon-color: rgba(var(--hum-rgb),1) !important;
                display: flex;
                margin: -18px 0 10px -20px !important;
                padding-right: 22px;
                padding-bottom: 25px;
              }
              ha-card {
                clip-path: inset(0 0 0 0 round var(--ha-card-border-radius, 14px));
                
                /* FONT SIZE & SPACING SETTINGS */
                --card-primary-font-size: 1.2rem !important;
                
                /* Increases vertical space between primary and secondary */
                --card-primary-line-height: 1.3 !important;
              }
      - type: custom:vertical-stack-in-card
        cards:
          - type: custom:mini-graph-card
            entities:
              - sensor.livingroom_humidity
            line_color: lightblue
            line_width: 5
            hours_to_show: 24
            show:
              name: false
              icon: false
              state: false
              labels: false
              legend: false
            card_mod:
              style: |
                ha-card {
                  background: none;
                  box-shadow: none;
                  opacity: 50%;
                  border: none;
                  width: 250px;
                  mask-image: radial-gradient(
                    ellipse at center,
                    rgba(0,0,0,1) 0%,
                    rgba(0,0,0,0) 90%
                  );
                }
        card_mod:
          style:
            .: |
              ha-card {
                  background: none;
                  box-shadow: none;
                  border: none;
                  margin: 8px 12px; 
                  position: absolute;
                  bottom: -10px;
                  right: -10px;
                  }
      - type: custom:mushroom-chips-card
        chips:
          - type: template
            entity: sensor.livingroom_mi_battery
            icon: |-
              {% if states(entity)|float<=20 %}
                mdi:battery-20
              {% elif states(entity)|float<=50 %} 
                mdi:battery-50
              {% else %} 
                mdi:battery
              {% endif %}
            content: "{{states(entity)|float|round(0)}}%"
            icon_color: |-
              {% if states(entity)|float<=25 %}
                red
              {% elif states(entity)|float<=75 %} 
                orange
              {% else %} 
                green
              {% endif %}
        alignment: end
        card_mod:
          style: >
            ha-card {
              /* ================================== */
              /* USER CONFIG HERE                   */
              /* ================================== */

              /* 1. ACTIVE STATE (NUMERIC) */
              {% set threshold_below = 20 %}
              {% set threshold_above = false %}

              /* 2. POSITIONING */
              {% set pos_y = 'top' %}               
              {% set pos_y_val = '10px' %}
              {% set pos_x = 'right' %}             
              {% set pos_x_val = '0%' %}

              /* 3. SIZING */
              {% set icon_size = '10px' %}          
              {% set text_size = '8px' %}          
              {% set badge_height = '3px' %}       
              {% set border_radius = '7px' %}       
              {% set border_thickness = '1px' %}
              
              /* 4. COLORS (HEX, RGB, Color Name) */
              /* --- Active --- */
              {% set active_bg = 'grey' %} 
              {% set active_icon = 'white' %}
              {% set active_text = 'darkgrey' %}
              {% set active_border = 'transparent' %}
              {% set active_opacity = 0.3 %}
              
              /* --- Inactive --- */
              {% set inactive_bg = 'grey' %}
              {% set inactive_icon = 'red' %}
              {% set inactive_text = 'white' %}
              {% set inactive_border = 'transparent' %}
              {% set inactive_opacity = 0.3 %}

              /* 5. ANIMATIONS */
              /* Options: pulse, shake, wobble   */
              /* flash, heartbeat, swing, vibrate  */
              
              {% set anim_type = 'flash' %}

              /* true = 24/7, false = only when active */
              {% set always_animate = false %}
              
              {% set anim_intensity_sec = 0.5 %} /* 0.1 to 1  */
              {% set anim_duration_sec = 2 %}
              {% set anim_delay_sec = 5 %}

              /* ================================== */
              /* USER CONFIG END (DONT TOUCH BELOW) */
              /* ================================== */

              {% set current_state = states(config.chips[0].entity) %}
              {% set state_num = current_state | float(-99999) %}
              
              {% set check_below = threshold_below | string | lower != 'false' %}
              {% set check_above = threshold_above | string | lower != 'false' %}
              
              {% set is_active_below = check_below and state_num != -99999 and state_num <= (threshold_below | float(-99999)) %}
              {% set is_active_above = check_above and state_num != -99999 and state_num >= (threshold_above | float(-99999)) %}
              
              {% set is_active = is_active_below or is_active_above %}
              
              {% if is_active %}
                {% set current_bg = active_bg %}
                {% set current_icon = active_icon %}
                {% set current_text = active_text %}
                {% set current_border = active_border %}
                {% set current_op = active_opacity %}
                {% set current_anim = anim_type %}
              {% else %}
                {% set current_bg = inactive_bg %}
                {% set current_icon = inactive_icon %}
                {% set current_text = inactive_text %}
                {% set current_border = inactive_border %}
                {% set current_op = inactive_opacity %}
                {% set current_anim = anim_type if always_animate else 'none' %}
              {% endif %}

              {% set master_cycle = anim_duration_sec + anim_delay_sec %}
              {% set iterations = (anim_duration_sec / anim_intensity_sec) | round(0, 'floor') | int if anim_intensity_sec > 0 else 0 %}
              {% set step_percent = (anim_intensity_sec / master_cycle) * 100 if master_cycle > 0 else 0 %}

              --chip-padding: {{ badge_height }};
              --chip-border-radius: {{ border_radius }};
              
              --chip-border-width: 0px !important;
              --ha-card-border-width: 0px !important;
              
              --chip-box-shadow: 0 0 0 {{ border_thickness }} {{ current_border }} !important;

              --text-color: {{ current_icon }} !important;
              --color: {{ current_icon }} !important; 
              --chip-background: color-mix(in srgb, {{ current_bg }}, transparent {{ (1 - current_op) * 100 }}%) !important;
              --chip-font-size: {{ text_size }} !important;
              --chip-icon-size: {{ icon_size }} !important; 
              --mdc-icon-size: {{ icon_size }} !important;   
              --chip-height: auto !important;
              --primary-text-color: {{ current_text }} !important;
              --secondary-text-color: {{ current_text }} !important;

              position: absolute !important;
              {{ pos_y }}: {{ pos_y_val }};
              {{ pos_x }}: {{ pos_x_val }};
              z-index: 1;
              border: none !important;
              border-radius: {{ border_radius }} !important;
              box-shadow: 0 4px 6px rgba(0,0,0,0.2);
              overflow: visible !important;
              will-change: transform; 
              
              {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}
                animation: anim-{{ config.chips[0].entity | replace('.', '-') }}-{{ anim_type }} {{ master_cycle }}s infinite linear;
              {% endif %}
            }


            ha-state-icon, ha-icon {
              display: flex;
              align-items: center;
              justify-content: center;
              width: {{ icon_size }} !important; 
              height: {{ icon_size }} !important;
            }


            {% if config.chips[0].content is not defined or
            config.chips[0].content | length == 0 %}
               ha-card { padding: 5px !important; border-radius: 50% !important; aspect-ratio: 1/1; justify-content: center; }
               .content { display: none; }
            {% else %}
               ha-card { padding: 0 10px !important; }
               .content { padding-left: 5px; font-weight: bold; }
            {% endif %}


            {% if current_anim != 'none' and iterations > 0 and master_cycle > 0
            %}

            @keyframes anim-{{ config.chips[0].entity | replace('.', '-') }}-{{
            anim_type }} {
              {% for i in range(iterations) %}
                {% set t = i * step_percent %} 
                
                {% if anim_type == 'pulse' %}
                  {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
                  {{ t + (step_percent * 0.5) }}% { transform: scale3d(1.15, 1.15, 1); }
                  {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

                {% elif anim_type == 'shake' %}
                  {{ t + 0 }}% { transform: rotateZ(0deg); }
                  {{ t + (step_percent * 0.25) }}% { transform: rotateZ(-10deg); }
                  {{ t + (step_percent * 0.75) }}% { transform: rotateZ(10deg); }
                  {{ t + step_percent }}% { transform: rotateZ(0deg); }

                {% elif anim_type == 'flash' %}
                  {{ t + 0 }}% { opacity: 1; }
                  {{ t + (step_percent * 0.5) }}% { opacity: 0; }
                  {{ t + step_percent }}% { opacity: 1; }

                {% elif anim_type == 'wobble' %}
                  {{ t + 0 }}% { transform: translate3d(0%, 0, 0); }
                  {{ t + (step_percent * 0.3) }}% { transform: translate3d(-15%, 0, 0) rotateZ(-5deg); }
                  {{ t + (step_percent * 0.6) }}% { transform: translate3d(10%, 0, 0) rotateZ(3deg); }
                  {{ t + step_percent }}% { transform: translate3d(0%, 0, 0); }

                {% elif anim_type == 'heartbeat' %}
                  {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
                  {{ t + (step_percent * 0.25) }}% { transform: scale3d(1.15, 1.15, 1); }
                  {{ t + (step_percent * 0.5) }}% { transform: scale3d(1, 1, 1); }
                  {{ t + (step_percent * 0.75) }}% { transform: scale3d(1.15, 1.15, 1); }
                  {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

                {% elif anim_type == 'swing' %}
                  {{ t + 0 }}% { transform: rotateZ(0deg); }
                  {{ t + (step_percent * 0.4) }}% { transform: rotateZ(15deg); }
                  {{ t + (step_percent * 0.8) }}% { transform: rotateZ(-10deg); }
                  {{ t + step_percent }}% { transform: rotateZ(0deg); }

                {% elif anim_type == 'vibrate' %}
                  {{ t + 0 }}% { transform: translate3d(0, 0, 0); }
                  {{ t + (step_percent * 0.2) }}% { transform: translate3d(-2px, 2px, 0); }
                  {{ t + (step_percent * 0.4) }}% { transform: translate3d(-2px, -2px, 0); }
                  {{ t + (step_percent * 0.6) }}% { transform: translate3d(2px, 2px, 0); }
                  {{ t + (step_percent * 0.8) }}% { transform: translate3d(2px, -2px, 0); }
                  {{ t + step_percent }}% { transform: translate3d(0, 0, 0); }

                {% endif %}
              {% endfor %}
            }

            {% endif %}

```
</details>

<img width="503" height="103" alt="Screenshot 2026-02-27 155908" src="https://github.com/user-attachments/assets/a9e38b6f-a5f2-4d61-a360-97ff514d790c" />

<details>
<summary><strong>Motion Sensor</summary>

```yaml
type: custom:vertical-stack-in-card
cards:
  - type: custom:mushroom-entity-card
    entity: binary_sensor.pir_gate_occupancy
    name: Motion Sensor
    icon: mdi:motion-sensor
    primary_info: name
    secondary_info: state
    tap_action:
      action: more-info
    icon_color: light-grey
    card_mod:
      style:
        .: |
          ha-card {
            /* ========================== */
            /* USER CONFIG                */
            /* ========================== */
            
            /* 1. What state the animation? */
            {% set active_state = 'Detected' %}

            /* 2. Icon Size */
            --custom-icon-size: 68px;

            /* 3. Colors (RGB) */
            {% set color_detected = '255, 50, 50' %} /* Redd */
            {% set color_scanning = '0, 200, 255' %} /* Blue */
            
            /* 4. Badge Text */
            {% set text_detected = 'DETECTED' %}
            {% set text_scanning = 'SCANNING' %}
            
            /* ========================== */

            {% set is_active = states(config.entity) == active_state %}

            {% if is_active %}
               {% set color = color_detected %}
               {% set badge = text_detected %}
               {% set bg_image = 'radial-gradient(circle at center, rgba('~color~', 0.15) 0%, transparent 70%)' %}
            {% else %}
               {% set color = color_scanning %}
               {% set badge = text_scanning %}
               {% set bg_image = 'none' %}
            {% endif %}

            /* --- CSS VARIABLES PASSING --- */
            --sonar-color: {{ color }};
            --sonar-bg: {{ bg_image }};
            --badge-text: "{{ badge }}";
            
            /* Animation Selectors */
            --anim-sweep: {{ 'block' if not is_active else 'none' }};
            --anim-pulse: {{ 'block' if is_active else 'none' }};
            
            /* --- CORE CARD STYLING --- */

            background-image: var(--sonar-bg) !important;
            transition: background-image 0.5s ease;
            border-radius: 12px;
            border: none;
            overflow: hidden;
          }

          /* --- STATUS BADGE (Top Right) --- */
          ha-card::before {
            content: var(--badge-text);
            position: absolute;
            top: 10px; right: 10px;
            font-size: 10px;
            font-weight: 900;
            letter-spacing: 1px;
            color: rgb(var(--sonar-color));
            background: rgba(var(--sonar-color), 0.15);
            border: 1px solid rgba(var(--sonar-color), 0.3);
            box-shadow: 0 0 10px rgba(var(--sonar-color), 0.2);
            padding: 3px 8px;
            border-radius: 6px;
            z-index: 2;
          }

          /* --- BOTTOM SCAN BAR --- */
          ha-card::after {
            content: '';
            position: absolute;
            bottom: 0; left: 0; right: 0;
            height: 4px;
            background: rgb(var(--sonar-color));
            box-shadow: 0 0 15px rgb(var(--sonar-color));
            opacity: {{ '1' if is_active else '0.5' }};
            transition: all 0.5s ease;
          }
        mushroom-shape-icon$: |
          .shape {
            /* PASS VARIABLES */
            --shape-color: var(--sonar-color);
            --icon-size: var(--custom-icon-size) !important;
            
            /* GEOMETRY FIXES */
            width: var(--icon-size) !important;
            height: var(--icon-size) !important;
            border-radius: 50% !important;
            background: transparent !important;
            
            /* Positioning */
            position: relative;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: visible !important;
            
            /* Static "HUD" Ring */
            border: 2px solid rgba(var(--shape-color), 0.2);
            box-shadow: inset 0 0 15px rgba(var(--shape-color), 0.1);
            transition: border-color 0.3s ease;
          }

          /* --- ANIMATION: RADAR SWEEP (SCANNING) --- */
          .shape::before {
            content: '';
            display: var(--anim-sweep);
            position: absolute;
            inset: -2px; /* Cover the border */
            border-radius: 50%;
            background: conic-gradient(
              from 0deg,
              transparent 0deg,
              transparent 270deg,
              rgba(var(--shape-color), 0.1) 280deg, 
              rgba(var(--shape-color), 1) 360deg
            );
            animation: radar-spin 2.5s linear infinite;
            z-index: 1;
          }

          /* --- ANIMATION: SHOCKWAVE (DETECTED) --- */
          .shape::after {
            content: '';
            display: var(--anim-pulse);
            position: absolute;
            inset: 0;
            border-radius: 50%;
            z-index: 0;
            animation: sonar-shockwave 2s infinite;
          }

          /* ICON STYLING */
          ha-icon {
            position: relative;
            z-index: 5;
            color: rgba(var(--shape-color), 1) !important;
            filter: drop-shadow(0 0 8px rgba(var(--shape-color), 0.8));
            transition: color 0.3s ease;
          }

          /* --- KEYFRAMES --- */
          @keyframes radar-spin {
            from { transform: rotate(0deg); }
            to   { transform: rotate(360deg); }
          }

          @keyframes sonar-shockwave {
            0% { box-shadow: 0 0 0 0 rgba(var(--shape-color), 0.8), 0 0 0 0 rgba(var(--shape-color), 0.8); }
            40% { box-shadow: 0 0 0 20px rgba(var(--shape-color), 0.3), 0 0 0 0 rgba(var(--shape-color), 0.8); }
            80% { box-shadow: 0 0 0 40px rgba(var(--shape-color), 0), 0 0 0 20px rgba(var(--shape-color), 0); }
            100% { box-shadow: 0 0 0 0 rgba(var(--shape-color), 0), 0 0 0 0 rgba(var(--shape-color), 0); }
          }
  - type: custom:mushroom-chips-card
    chips:
      - type: template
        entity: sensor.pir_gate_battery
        tap_action:
          action: none
        icon: |-
          {% if states(entity)|float<=20 %}
            mdi:battery-20
          {% elif states(entity)|float<=50 %} 
            mdi:battery-50
          {% elif states(entity)|float<=75 %} 
            mdi:battery-70
          {% else %} 
            mdi:battery
          {% endif %}
        content: "{{states(entity)|float|round(0)}}%"
        hold_action:
          action: none
        double_tap_action:
          action: none
        icon_color: |-
          {% if states(entity)|float<=20 %}
            red
          {% elif states(entity)|float<=50 %} 
            yellow
          {% elif states(entity)|float<=75 %} 
            orange
          {% else %} 
            green
          {% endif %}
    alignment: end
    card_mod:
      style: >
        ha-card {
          /* ================================== */
          /* USER CONFIG HERE                   */
          /* ================================== */

          /* 1. ACTIVE STATE (NUMERIC) */
          {% set threshold_below = 20 %}
          {% set threshold_above = false %}

          /* 2. POSITIONING */
          {% set pos_y = 'top' %}               
          {% set pos_y_val = '10px' %}
          {% set pos_x = 'right' %}             
          {% set pos_x_val = '85px' %}

          /* 3. SIZING */
          {% set icon_size = '17px' %}          
          {% set text_size = '11px' %}          
          {% set badge_height = '4px' %}       
          {% set border_radius = '5px' %}       
          {% set border_thickness = '1px' %}
          
          /* 4. COLORS (HEX, RGB, Color Name) */
          /* --- Active --- */
          {% set active_bg = 'grey' %} 
          {% set active_icon = 'white' %}
          {% set active_text = 'darkgrey' %}
          {% set active_border = 'transparent' %}
          {% set active_opacity = 0.3 %}
          
          /* --- Inactive --- */
          {% set inactive_bg = 'grey' %}
          {% set inactive_icon = 'red' %}
          {% set inactive_text = 'white' %}
          {% set inactive_border = 'transparent' %}
          {% set inactive_opacity = 0.3 %}

          /* 5. ANIMATIONS */
          /* Options: pulse, shake, wobble   */
          /* flash, heartbeat, swing, vibrate  */
          
          {% set anim_type = 'flash' %}

          /* true = 24/7, false = only when active */
          {% set always_animate = false %}
          
          {% set anim_intensity_sec = 0.5 %} /* 0.1 to 1  */
          {% set anim_duration_sec = 2 %}
          {% set anim_delay_sec = 5 %}

          /* ================================== */
          /* USER CONFIG END (DONT TOUCH BELOW) */
          /* ================================== */

          {% set current_state = states(config.chips[0].entity) %}
          {% set state_num = current_state | float(-99999) %}
          
          {% set check_below = threshold_below | string | lower != 'false' %}
          {% set check_above = threshold_above | string | lower != 'false' %}
          
          {% set is_active_below = check_below and state_num != -99999 and state_num <= (threshold_below | float(-99999)) %}
          {% set is_active_above = check_above and state_num != -99999 and state_num >= (threshold_above | float(-99999)) %}
          
          {% set is_active = is_active_below or is_active_above %}
          
          {% if is_active %}
            {% set current_bg = active_bg %}
            {% set current_icon = active_icon %}
            {% set current_text = active_text %}
            {% set current_border = active_border %}
            {% set current_op = active_opacity %}
            {% set current_anim = anim_type %}
          {% else %}
            {% set current_bg = inactive_bg %}
            {% set current_icon = inactive_icon %}
            {% set current_text = inactive_text %}
            {% set current_border = inactive_border %}
            {% set current_op = inactive_opacity %}
            {% set current_anim = anim_type if always_animate else 'none' %}
          {% endif %}

          {% set master_cycle = anim_duration_sec + anim_delay_sec %}
          {% set iterations = (anim_duration_sec / anim_intensity_sec) | round(0, 'floor') | int if anim_intensity_sec > 0 else 0 %}
          {% set step_percent = (anim_intensity_sec / master_cycle) * 100 if master_cycle > 0 else 0 %}

          --chip-padding: {{ badge_height }};
          --chip-border-radius: {{ border_radius }};
          
          --chip-border-width: 0px !important;
          --ha-card-border-width: 0px !important;
          
          --chip-box-shadow: 0 0 0 {{ border_thickness }} {{ current_border }} !important;

          --text-color: {{ current_icon }} !important;
          --color: {{ current_icon }} !important; 
          --chip-background: color-mix(in srgb, {{ current_bg }}, transparent {{ (1 - current_op) * 100 }}%) !important;
          --chip-font-size: {{ text_size }} !important;
          --chip-icon-size: {{ icon_size }} !important; 
          --mdc-icon-size: {{ icon_size }} !important;   
          --chip-height: auto !important;
          --primary-text-color: {{ current_text }} !important;
          --secondary-text-color: {{ current_text }} !important;

          position: absolute !important;
          {{ pos_y }}: {{ pos_y_val }};
          {{ pos_x }}: {{ pos_x_val }};
          z-index: 1;
          border: none !important;
          border-radius: {{ border_radius }} !important;
          box-shadow: 0 4px 6px rgba(0,0,0,0.2);
          overflow: visible !important;
          will-change: transform; 
          
          {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}
            animation: anim-{{ config.chips[0].entity | replace('.', '-') }}-{{ anim_type }} {{ master_cycle }}s infinite linear;
          {% endif %}
        }


        ha-state-icon, ha-icon {
          display: flex;
          align-items: center;
          justify-content: center;
          width: {{ icon_size }} !important; 
          height: {{ icon_size }} !important;
        }


        {% if config.chips[0].content is not defined or config.chips[0].content
        | length == 0 %}
           ha-card { padding: 5px !important; border-radius: 50% !important; aspect-ratio: 1/1; justify-content: center; }
           .content { display: none; }
        {% else %}
           ha-card { padding: 0 10px !important; }
           .content { padding-left: 5px; font-weight: bold; }
        {% endif %}


        {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}

        @keyframes anim-{{ config.chips[0].entity | replace('.', '-') }}-{{
        anim_type }} {
          {% for i in range(iterations) %}
            {% set t = i * step_percent %} 
            
            {% if anim_type == 'pulse' %}
              {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.5) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

            {% elif anim_type == 'shake' %}
              {{ t + 0 }}% { transform: rotateZ(0deg); }
              {{ t + (step_percent * 0.25) }}% { transform: rotateZ(-10deg); }
              {{ t + (step_percent * 0.75) }}% { transform: rotateZ(10deg); }
              {{ t + step_percent }}% { transform: rotateZ(0deg); }

            {% elif anim_type == 'flash' %}
              {{ t + 0 }}% { opacity: 1; }
              {{ t + (step_percent * 0.5) }}% { opacity: 0; }
              {{ t + step_percent }}% { opacity: 1; }

            {% elif anim_type == 'wobble' %}
              {{ t + 0 }}% { transform: translate3d(0%, 0, 0); }
              {{ t + (step_percent * 0.3) }}% { transform: translate3d(-15%, 0, 0) rotateZ(-5deg); }
              {{ t + (step_percent * 0.6) }}% { transform: translate3d(10%, 0, 0) rotateZ(3deg); }
              {{ t + step_percent }}% { transform: translate3d(0%, 0, 0); }

            {% elif anim_type == 'heartbeat' %}
              {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.25) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + (step_percent * 0.5) }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.75) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

            {% elif anim_type == 'swing' %}
              {{ t + 0 }}% { transform: rotateZ(0deg); }
              {{ t + (step_percent * 0.4) }}% { transform: rotateZ(15deg); }
              {{ t + (step_percent * 0.8) }}% { transform: rotateZ(-10deg); }
              {{ t + step_percent }}% { transform: rotateZ(0deg); }

            {% elif anim_type == 'vibrate' %}
              {{ t + 0 }}% { transform: translate3d(0, 0, 0); }
              {{ t + (step_percent * 0.2) }}% { transform: translate3d(-2px, 2px, 0); }
              {{ t + (step_percent * 0.4) }}% { transform: translate3d(-2px, -2px, 0); }
              {{ t + (step_percent * 0.6) }}% { transform: translate3d(2px, 2px, 0); }
              {{ t + (step_percent * 0.8) }}% { transform: translate3d(2px, -2px, 0); }
              {{ t + step_percent }}% { transform: translate3d(0, 0, 0); }

            {% endif %}
          {% endfor %}
        }

        {% endif %}

```
</details>

<img width="501" height="126" alt="Screenshot 2026-02-27 155913" src="https://github.com/user-attachments/assets/1f910e41-10de-42dc-8629-7e69387f1aa2" />

<details>
<summary><strong>Light card</summary>

```yaml
type: custom:vertical-stack-in-card
cards:
  - type: custom:mushroom-light-card
    card_mod:
      style:
        mushroom-shape-icon$: ""
        .: |
          mushroom-shape-icon {
            --icon-size: 60px;
            display: flex;
            margin: -18px 0px 0px -18px !important;
          }
          ha-card {
            clip-path: inset(0 0 0 0 round var(--ha-card-border-radius, 12px));
          }
    entity: light.right_led
    fill_container: true
    use_light_color: true
    show_color_temp_control: true
    collapsible_controls: false
    show_color_control: true
    show_brightness_control: true
    tap_action:
      action: toggle
    icon: mdi:drag-vertical
    double_tap_action:
      action: none
    hold_action:
      action: more-info
    name: " Right LED"
  - type: custom:mushroom-chips-card
    chips:
      - type: template
        entity: sensor.your_entity_here
        icon: mdi:party-popper
        tap_action:
          action: perform-action
          perform_action: script.your_script_here
          target: {}
    alignment: end
    card_mod:
      style: >
        ha-card {
          /* ================================ */
          /* USER CONFIG HERE                 */
          /* ================================ */

          /* 1. ACTIVE STATES */
          {% set active_states = ['on', 'off'] %}
          
          /* 2. POSITIONING */
          {% set pos_y = 'top' %}               
          {% set pos_y_val = '5%' %}
          {% set pos_x = 'right' %}             
          {% set pos_x_val = '22%' %}

          /* 3. SIZING */
          {% set icon_size = '25px' %}          
          {% set text_size = '12px' %}          
          {% set badge_height = '7px' %}       
          {% set border_radius = '10px' %}      
          {% set border_thickness = '0.5px' %}
          
          /* 4. COLORS (HEX, RGB, Color Name) */
          /* --- Active --- */
          {% set active_bg = 'red' %} 
          {% set active_icon = 'white' %}
          {% set active_text = 'white' %}
          {% set active_border = 'white' %}
          {% set active_opacity = 0.3 %}
          
          /* --- Inactive --- */
          {% set inactive_bg = '#d980f5' %}
          {% set inactive_icon = 'white' %}
          {% set inactive_text = 'white' %}
          {% set inactive_border = 'transparent' %}
          {% set inactive_opacity = 0.3 %}
          

          /* 5. ANIMATIONS */
          /* Options: pulse, shake, wobble   */
          /* flash, heartbeat, swing, vibrate  */
          
          {% set anim_type = 'none' %}

          /* true = 24/7, false = only when active */
          {% set always_animate = false %}
          
          {% set anim_intensity_sec = 0.5 %} /* 0.1 to 1  */
          {% set anim_duration_sec = 1 %}
          {% set anim_delay_sec = 1 %}

          /* ================================== */
          /* USER CONFIG END (DONT TOUCH BELOW) */
          /* ================================== */

          {% set current_state = states(config.chips[0].entity) %}
          {% set is_active = current_state in active_states %}
          
          {% if is_active %}
            {% set current_bg = active_bg %}
            {% set current_icon = active_icon %}
            {% set current_text = active_text %}
            {% set current_border = active_border %}
            {% set current_op = active_opacity %}
            {% set current_anim = anim_type %}
          {% else %}
            {% set current_bg = inactive_bg %}
            {% set current_icon = inactive_icon %}
            {% set current_text = inactive_text %}
            {% set current_border = inactive_border %}
            {% set current_op = inactive_opacity %}
            {% set current_anim = anim_type if always_animate else 'none' %}
          {% endif %}

          {% set master_cycle = anim_duration_sec + anim_delay_sec %}
          {% set iterations = (anim_duration_sec / anim_intensity_sec) | round(0, 'floor') | int if anim_intensity_sec > 0 else 0 %}
          {% set step_percent = (anim_intensity_sec / master_cycle) * 100 if master_cycle > 0 else 0 %}

          --chip-padding: {{ badge_height }};
          --chip-border-radius: {{ border_radius }};
          
          --chip-border-width: 0px !important;
          --ha-card-border-width: 0px !important;
          
          --chip-box-shadow: 0 0 0 {{ border_thickness }} {{ current_border }} !important;

          --text-color: {{ current_icon }} !important;
          --color: {{ current_icon }} !important; 
          --chip-background: color-mix(in srgb, {{ current_bg }}, transparent {{ (1 - current_op) * 100 }}%) !important;
          --chip-font-size: {{ text_size }} !important;
          --chip-icon-size: {{ icon_size }} !important; 
          --mdc-icon-size: {{ icon_size }} !important;   
          --chip-height: auto !important;
          --primary-text-color: {{ current_text }} !important;
          --secondary-text-color: {{ current_text }} !important;

          position: absolute !important;
          {{ pos_y }}: {{ pos_y_val }};
          {{ pos_x }}: {{ pos_x_val }};
          z-index: 1;
          border: none !important;
          border-radius: {{ border_radius }} !important;
          box-shadow: 0 4px 6px rgba(0,0,0,0.2);
          overflow: visible !important;
          will-change: transform; 
          
          {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}
            animation: anim-{{ config.chips[0].entity | replace('.', '-') }}-{{ anim_type }} {{ master_cycle }}s infinite linear;
          {% endif %}
        }


        ha-state-icon, ha-icon {
          display: flex;
          align-items: center;
          justify-content: center;
          width: {{ icon_size }} !important; 
          height: {{ icon_size }} !important;
        }


        {% if config.chips[0].content is not defined or config.chips[0].content
        | length == 0 %}
           ha-card { padding: 5px !important; border-radius: 50% !important; aspect-ratio: 1/1; justify-content: center; }
           .content { display: none; }
        {% else %}
           ha-card { padding: 0 10px !important; }
           .content { padding-left: 5px; font-weight: bold; }
        {% endif %}


        {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}

        @keyframes anim-{{ config.chips[0].entity | replace('.', '-') }}-{{
        anim_type }} {
          {% for i in range(iterations) %}
            {% set t = i * step_percent %} 
            
            {% if anim_type == 'pulse' %}
              {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.5) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

            {% elif anim_type == 'shake' %}
              {{ t + 0 }}% { transform: rotateZ(0deg); }
              {{ t + (step_percent * 0.25) }}% { transform: rotateZ(-10deg); }
              {{ t + (step_percent * 0.75) }}% { transform: rotateZ(10deg); }
              {{ t + step_percent }}% { transform: rotateZ(0deg); }

            {% elif anim_type == 'flash' %}
              {{ t + 0 }}% { opacity: 1; }
              {{ t + (step_percent * 0.5) }}% { opacity: 0; }
              {{ t + step_percent }}% { opacity: 1; }

            {% elif anim_type == 'wobble' %}
              {{ t + 0 }}% { transform: translate3d(0%, 0, 0); }
              {{ t + (step_percent * 0.3) }}% { transform: translate3d(-15%, 0, 0) rotateZ(-5deg); }
              {{ t + (step_percent * 0.6) }}% { transform: translate3d(10%, 0, 0) rotateZ(3deg); }
              {{ t + step_percent }}% { transform: translate3d(0%, 0, 0); }

            {% elif anim_type == 'heartbeat' %}
              {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.25) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + (step_percent * 0.5) }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.75) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

            {% elif anim_type == 'swing' %}
              {{ t + 0 }}% { transform: rotateZ(0deg); }
              {{ t + (step_percent * 0.4) }}% { transform: rotateZ(15deg); }
              {{ t + (step_percent * 0.8) }}% { transform: rotateZ(-10deg); }
              {{ t + step_percent }}% { transform: rotateZ(0deg); }

            {% elif anim_type == 'vibrate' %}
              {{ t + 0 }}% { transform: translate3d(0, 0, 0); }
              {{ t + (step_percent * 0.2) }}% { transform: translate3d(-2px, 2px, 0); }
              {{ t + (step_percent * 0.4) }}% { transform: translate3d(-2px, -2px, 0); }
              {{ t + (step_percent * 0.6) }}% { transform: translate3d(2px, 2px, 0); }
              {{ t + (step_percent * 0.8) }}% { transform: translate3d(2px, -2px, 0); }
              {{ t + step_percent }}% { transform: translate3d(0, 0, 0); }

            {% endif %}
          {% endfor %}
        }

        {% endif %}
  - type: custom:mushroom-chips-card
    chips:
      - type: template
        entity: sensor.your_entity_here
        icon: mdi:fire
        tap_action:
          action: perform-action
          perform_action: script.your_script_here
          target: {}
    alignment: end
    card_mod:
      style: >
        ha-card {
          /* ================================ */
          /* USER CONFIG HERE                 */
          /* ================================ */

          /* 1. ACTIVE STATES */
          {% set active_states = ['on', 'off'] %}
          
          /* 2. POSITIONING */
          {% set pos_y = 'top' %}               
          {% set pos_y_val = '5%' %}
          {% set pos_x = 'right' %}             
          {% set pos_x_val = '12%' %}

          /* 3. SIZING */
          {% set icon_size = '25px' %}          
          {% set text_size = '12px' %}          
          {% set badge_height = '7px' %}       
          {% set border_radius = '10px' %}       
          {% set border_thickness = '0.5px' %}
          
          /* 4. COLORS (HEX, RGB, Color Name) */
          /* --- Active --- */
          {% set active_bg = 'red' %} 
          {% set active_icon = 'white' %}
          {% set active_text = 'white' %}
          {% set active_border = 'white' %}
          {% set active_opacity = 0.3 %}
          
          /* --- Inactive --- */
          {% set inactive_bg = '#ff7800' %}
          {% set inactive_icon = 'white' %}
          {% set inactive_text = 'white' %}
          {% set inactive_border = 'transparent' %}
          {% set inactive_opacity = 0.3 %}
          

          /* 5. ANIMATIONS */
          /* Options: pulse, shake, wobble   */
          /* flash, heartbeat, swing, vibrate  */
          
          {% set anim_type = 'none' %}

          /* true = 24/7, false = only when active */
          {% set always_animate = false %}
          
          {% set anim_intensity_sec = 0.5 %} /* 0.1 to 1  */
          {% set anim_duration_sec = 1 %}
          {% set anim_delay_sec = 1 %}

          /* ================================== */
          /* USER CONFIG END (DONT TOUCH BELOW) */
          /* ================================== */

          {% set current_state = states(config.chips[0].entity) %}
          {% set is_active = current_state in active_states %}
          
          {% if is_active %}
            {% set current_bg = active_bg %}
            {% set current_icon = active_icon %}
            {% set current_text = active_text %}
            {% set current_border = active_border %}
            {% set current_op = active_opacity %}
            {% set current_anim = anim_type %}
          {% else %}
            {% set current_bg = inactive_bg %}
            {% set current_icon = inactive_icon %}
            {% set current_text = inactive_text %}
            {% set current_border = inactive_border %}
            {% set current_op = inactive_opacity %}
            {% set current_anim = anim_type if always_animate else 'none' %}
          {% endif %}

          {% set master_cycle = anim_duration_sec + anim_delay_sec %}
          {% set iterations = (anim_duration_sec / anim_intensity_sec) | round(0, 'floor') | int if anim_intensity_sec > 0 else 0 %}
          {% set step_percent = (anim_intensity_sec / master_cycle) * 100 if master_cycle > 0 else 0 %}

          --chip-padding: {{ badge_height }};
          --chip-border-radius: {{ border_radius }};
          
          --chip-border-width: 0px !important;
          --ha-card-border-width: 0px !important;
          
          --chip-box-shadow: 0 0 0 {{ border_thickness }} {{ current_border }} !important;

          --text-color: {{ current_icon }} !important;
          --color: {{ current_icon }} !important; 
          --chip-background: color-mix(in srgb, {{ current_bg }}, transparent {{ (1 - current_op) * 100 }}%) !important;
          --chip-font-size: {{ text_size }} !important;
          --chip-icon-size: {{ icon_size }} !important; 
          --mdc-icon-size: {{ icon_size }} !important;   
          --chip-height: auto !important;
          --primary-text-color: {{ current_text }} !important;
          --secondary-text-color: {{ current_text }} !important;

          position: absolute !important;
          {{ pos_y }}: {{ pos_y_val }};
          {{ pos_x }}: {{ pos_x_val }};
          z-index: 1;
          border: none !important;
          border-radius: {{ border_radius }} !important;
          box-shadow: 0 4px 6px rgba(0,0,0,0.2);
          overflow: visible !important;
          will-change: transform; 
          
          {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}
            animation: anim-{{ config.chips[0].entity | replace('.', '-') }}-{{ anim_type }} {{ master_cycle }}s infinite linear;
          {% endif %}
        }


        ha-state-icon, ha-icon {
          display: flex;
          align-items: center;
          justify-content: center;
          width: {{ icon_size }} !important; 
          height: {{ icon_size }} !important;
        }


        {% if config.chips[0].content is not defined or config.chips[0].content
        | length == 0 %}
           ha-card { padding: 5px !important; border-radius: 50% !important; aspect-ratio: 1/1; justify-content: center; }
           .content { display: none; }
        {% else %}
           ha-card { padding: 0 10px !important; }
           .content { padding-left: 5px; font-weight: bold; }
        {% endif %}


        {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}

        @keyframes anim-{{ config.chips[0].entity | replace('.', '-') }}-{{
        anim_type }} {
          {% for i in range(iterations) %}
            {% set t = i * step_percent %} 
            
            {% if anim_type == 'pulse' %}
              {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.5) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

            {% elif anim_type == 'shake' %}
              {{ t + 0 }}% { transform: rotateZ(0deg); }
              {{ t + (step_percent * 0.25) }}% { transform: rotateZ(-10deg); }
              {{ t + (step_percent * 0.75) }}% { transform: rotateZ(10deg); }
              {{ t + step_percent }}% { transform: rotateZ(0deg); }

            {% elif anim_type == 'flash' %}
              {{ t + 0 }}% { opacity: 1; }
              {{ t + (step_percent * 0.5) }}% { opacity: 0; }
              {{ t + step_percent }}% { opacity: 1; }

            {% elif anim_type == 'wobble' %}
              {{ t + 0 }}% { transform: translate3d(0%, 0, 0); }
              {{ t + (step_percent * 0.3) }}% { transform: translate3d(-15%, 0, 0) rotateZ(-5deg); }
              {{ t + (step_percent * 0.6) }}% { transform: translate3d(10%, 0, 0) rotateZ(3deg); }
              {{ t + step_percent }}% { transform: translate3d(0%, 0, 0); }

            {% elif anim_type == 'heartbeat' %}
              {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.25) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + (step_percent * 0.5) }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.75) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

            {% elif anim_type == 'swing' %}
              {{ t + 0 }}% { transform: rotateZ(0deg); }
              {{ t + (step_percent * 0.4) }}% { transform: rotateZ(15deg); }
              {{ t + (step_percent * 0.8) }}% { transform: rotateZ(-10deg); }
              {{ t + step_percent }}% { transform: rotateZ(0deg); }

            {% elif anim_type == 'vibrate' %}
              {{ t + 0 }}% { transform: translate3d(0, 0, 0); }
              {{ t + (step_percent * 0.2) }}% { transform: translate3d(-2px, 2px, 0); }
              {{ t + (step_percent * 0.4) }}% { transform: translate3d(-2px, -2px, 0); }
              {{ t + (step_percent * 0.6) }}% { transform: translate3d(2px, 2px, 0); }
              {{ t + (step_percent * 0.8) }}% { transform: translate3d(2px, -2px, 0); }
              {{ t + step_percent }}% { transform: translate3d(0, 0, 0); }

            {% endif %}
          {% endfor %}
        }

        {% endif %}
  - type: custom:mushroom-chips-card
    chips:
      - type: template
        entity: sensor.your_entity_here
        icon: mdi:white-balance-auto
        tap_action:
          action: perform-action
          perform_action: script.your_script_here
          target: {}
    alignment: end
    card_mod:
      style: >
        ha-card {
          /* ================================ */
          /* USER CONFIG HERE                 */
          /* ================================ */

          /* 1. ACTIVE STATES */
          {% set active_states = ['on', 'off'] %}
          
          /* 2. POSITIONING */
          {% set pos_y = 'top' %}               
          {% set pos_y_val = '5%' %}
          {% set pos_x = 'right' %}             
          {% set pos_x_val = '2%' %}

          /* 3. SIZING */
          {% set icon_size = '25px' %}          
          {% set text_size = '12px' %}          
          {% set badge_height = '7px' %}       
          {% set border_radius = '10px' %}       
          {% set border_thickness = '0.5px' %}
          
          /* 4. COLORS (HEX, RGB, Color Name) */
          /* --- Active --- */
          {% set active_bg = 'red' %} 
          {% set active_icon = 'white' %}
          {% set active_text = 'white' %}
          {% set active_border = 'white' %}
          {% set active_opacity = 0.3 %}
          
          /* --- Inactive --- */
          {% set inactive_bg = '#f5d45c' %}
          {% set inactive_icon = 'white' %}
          {% set inactive_text = 'white' %}
          {% set inactive_border = 'transparent' %}
          {% set inactive_opacity = 0.3 %}
          

          /* 5. ANIMATIONS */
          /* Options: pulse, shake, wobble   */
          /* flash, heartbeat, swing, vibrate  */
          
          {% set anim_type = 'none' %}

          /* true = 24/7, false = only when active */
          {% set always_animate = false %}
          
          {% set anim_intensity_sec = 0.5 %} /* 0.1 to 1  */
          {% set anim_duration_sec = 1 %}
          {% set anim_delay_sec = 1 %}

          /* ================================== */
          /* USER CONFIG END (DONT TOUCH BELOW) */
          /* ================================== */

          {% set current_state = states(config.chips[0].entity) %}
          {% set is_active = current_state in active_states %}
          
          {% if is_active %}
            {% set current_bg = active_bg %}
            {% set current_icon = active_icon %}
            {% set current_text = active_text %}
            {% set current_border = active_border %}
            {% set current_op = active_opacity %}
            {% set current_anim = anim_type %}
          {% else %}
            {% set current_bg = inactive_bg %}
            {% set current_icon = inactive_icon %}
            {% set current_text = inactive_text %}
            {% set current_border = inactive_border %}
            {% set current_op = inactive_opacity %}
            {% set current_anim = anim_type if always_animate else 'none' %}
          {% endif %}

          {% set master_cycle = anim_duration_sec + anim_delay_sec %}
          {% set iterations = (anim_duration_sec / anim_intensity_sec) | round(0, 'floor') | int if anim_intensity_sec > 0 else 0 %}
          {% set step_percent = (anim_intensity_sec / master_cycle) * 100 if master_cycle > 0 else 0 %}

          --chip-padding: {{ badge_height }};
          --chip-border-radius: {{ border_radius }};
          
          --chip-border-width: 0px !important;
          --ha-card-border-width: 0px !important;
          
          --chip-box-shadow: 0 0 0 {{ border_thickness }} {{ current_border }} !important;

          --text-color: {{ current_icon }} !important;
          --color: {{ current_icon }} !important; 
          --chip-background: color-mix(in srgb, {{ current_bg }}, transparent {{ (1 - current_op) * 100 }}%) !important;
          --chip-font-size: {{ text_size }} !important;
          --chip-icon-size: {{ icon_size }} !important; 
          --mdc-icon-size: {{ icon_size }} !important;   
          --chip-height: auto !important;
          --primary-text-color: {{ current_text }} !important;
          --secondary-text-color: {{ current_text }} !important;

          position: absolute !important;
          {{ pos_y }}: {{ pos_y_val }};
          {{ pos_x }}: {{ pos_x_val }};
          z-index: 1;
          border: none !important;
          border-radius: {{ border_radius }} !important;
          box-shadow: 0 4px 6px rgba(0,0,0,0.2);
          overflow: visible !important;
          will-change: transform; 
          
          {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}
            animation: anim-{{ config.chips[0].entity | replace('.', '-') }}-{{ anim_type }} {{ master_cycle }}s infinite linear;
          {% endif %}
        }


        ha-state-icon, ha-icon {
          display: flex;
          align-items: center;
          justify-content: center;
          width: {{ icon_size }} !important; 
          height: {{ icon_size }} !important;
        }


        {% if config.chips[0].content is not defined or config.chips[0].content
        | length == 0 %}
           ha-card { padding: 5px !important; border-radius: 50% !important; aspect-ratio: 1/1; justify-content: center; }
           .content { display: none; }
        {% else %}
           ha-card { padding: 0 10px !important; }
           .content { padding-left: 5px; font-weight: bold; }
        {% endif %}


        {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}

        @keyframes anim-{{ config.chips[0].entity | replace('.', '-') }}-{{
        anim_type }} {
          {% for i in range(iterations) %}
            {% set t = i * step_percent %} 
            
            {% if anim_type == 'pulse' %}
              {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.5) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

            {% elif anim_type == 'shake' %}
              {{ t + 0 }}% { transform: rotateZ(0deg); }
              {{ t + (step_percent * 0.25) }}% { transform: rotateZ(-10deg); }
              {{ t + (step_percent * 0.75) }}% { transform: rotateZ(10deg); }
              {{ t + step_percent }}% { transform: rotateZ(0deg); }

            {% elif anim_type == 'flash' %}
              {{ t + 0 }}% { opacity: 1; }
              {{ t + (step_percent * 0.5) }}% { opacity: 0; }
              {{ t + step_percent }}% { opacity: 1; }

            {% elif anim_type == 'wobble' %}
              {{ t + 0 }}% { transform: translate3d(0%, 0, 0); }
              {{ t + (step_percent * 0.3) }}% { transform: translate3d(-15%, 0, 0) rotateZ(-5deg); }
              {{ t + (step_percent * 0.6) }}% { transform: translate3d(10%, 0, 0) rotateZ(3deg); }
              {{ t + step_percent }}% { transform: translate3d(0%, 0, 0); }

            {% elif anim_type == 'heartbeat' %}
              {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.25) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + (step_percent * 0.5) }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.75) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

            {% elif anim_type == 'swing' %}
              {{ t + 0 }}% { transform: rotateZ(0deg); }
              {{ t + (step_percent * 0.4) }}% { transform: rotateZ(15deg); }
              {{ t + (step_percent * 0.8) }}% { transform: rotateZ(-10deg); }
              {{ t + step_percent }}% { transform: rotateZ(0deg); }

            {% elif anim_type == 'vibrate' %}
              {{ t + 0 }}% { transform: translate3d(0, 0, 0); }
              {{ t + (step_percent * 0.2) }}% { transform: translate3d(-2px, 2px, 0); }
              {{ t + (step_percent * 0.4) }}% { transform: translate3d(-2px, -2px, 0); }
              {{ t + (step_percent * 0.6) }}% { transform: translate3d(2px, 2px, 0); }
              {{ t + (step_percent * 0.8) }}% { transform: translate3d(2px, -2px, 0); }
              {{ t + step_percent }}% { transform: translate3d(0, 0, 0); }

            {% endif %}
          {% endfor %}
        }

        {% endif %}

```
</details>

<img width="501" height="100" alt="Screenshot 2026-02-27 155956" src="https://github.com/user-attachments/assets/9bd8e4b4-72ca-4e6a-8ff5-46b2c7cbbe5b" />

<details>
<summary><strong>Mail card</summary>

```yaml
type: custom:vertical-stack-in-card
cards:
  - type: custom:mushroom-entity-card
    entity: input_boolean.mail_arrived
    name: Mailbox
    icon: mdi:mailbox
    secondary_info: none
    tap_action:
      action: none
    icon_color: orange
    primary_info: name
    card_mod:
      style:
        .: |
          ha-card {
            /* ========================== */
            /*  USER CONFIG               */
            /* ========================== */
            
            /* 1. Define ACTIVE State */
            {% set active_state = 'on' %}

            /* 2. icon size */
            --set-icon-size: 68px;

            /* 3. Custom Badge TEXT? */
            {% set active_badge_text = 'YOU\'VE GOT MAIL' %}
            {% set empty_badge_text = 'NO MAIL' %}

            /* 4. Custom color? (RGB) */
            {% set color_active = '255, 190, 50' %}   /* Orange */
            {% set color_empty  = '158, 158, 158' %} /* Grey */

            /* ========================== */

            {% set has_mail = states(config.entity) == active_state %}
            
            /* 2. Status & Colors */
            {% if has_mail %}
               {% set state = 'mail' %}
               {% set color = color_active %} 
               {% set badge = active_badge_text %}
            {% else %}
               {% set state = 'empty' %}
               {% set color = color_empty %} 
               {% set badge = empty_badge_text %}
            {% endif %}
            
            /* CSS VARS */
            --mb-color: {{ color }};
            --mb-badge: "{{ badge }}";
            
            /* Animation Selectors */
            --anim-mail-drop: {{ 'block' if state == 'mail' else 'none' }};
            --icon-move: {{ 'flag-wave 2s ease-in-out infinite' if state == 'mail' else 'none' }};
            
            /* --- CORE CARD STYLING --- */
            background: var(--card-background-color, #1c1c1c);
            border-radius: var(--ha-card-border-radius, 12px) !important;
            
            /* Keeps the bar inside the rounded corners */
            overflow: hidden; 
            
            transition: all 0.5s ease;
            position: relative;
          }



          /* --- BOTTOM HIGHLIGHT BAR --- */
          ha-card::after {
            content: '';
            position: absolute;
            bottom: 0; 
            left: 0;
            height: 4px; 
            width: 100%;
            
            /* Dynamic Colors based on State */
            background: rgb(var(--mb-color));
            box-shadow: 0 0 15px rgb(var(--mb-color));
            
            transition: background 0.5s ease, box-shadow 0.5s ease;
            opacity: 1; /* Always visible now */
            z-index: 1;
          }
        mushroom-shape-icon$: |
          .shape {
            background: rgba(var(--mb-color), 0.15) !important;
            --icon-size: var(--set-icon-size) !important;
            position: relative;
            overflow: hidden; /* Hide the letter before it drops in */
            transition: background 0.3s ease;
          }

          /* -------------------------------------- */
          /* ANIMATION: THE BIG FALLING LETTER      */
          /* -------------------------------------- */
          .shape::before {
            content: '';
            display: var(--anim-mail-drop);
            position: absolute;
            
            /* --- BIGGER ENVELOPE SIZE --- */
            width: 28px; height: 18px;
            
            /* White Envelope Body */
            background: #fff;
            border-radius: 3px;
            
            /* Center it horizontally */
            left: calc(50% - 14px);
            
            /* The Envelope Flap (Triangle) details */
            background-image: linear-gradient(135deg, transparent 50%, #ccc 50%), 
                              linear-gradient(225deg, transparent 50%, #ccc 50%);
            background-size: 50% 100%;
            background-repeat: no-repeat;
            background-position: 0 0, 100% 0;
            
            box-shadow: 0 4px 6px rgba(0,0,0,0.4);
            
            /* Run Animation */
            animation: mail-drop 1.5s ease-in-out infinite;
          }

          /* -------------------------------------- */
          /* ANIMATION: ICON MOVEMENT               */
          /* -------------------------------------- */
          ha-icon {
            color: rgb(var(--mb-color)) !important;
            transition: color 0.3s ease;
            animation: var(--icon-move);
            z-index: 2; 
          }

          /* --- KEYFRAMES --- */

          @keyframes mail-drop {
            0%   { top: -30px; opacity: 0; transform: rotate(-10deg); }
            30%  { opacity: 1; transform: rotate(0deg); }
            60%  { top: 50%; opacity: 1; transform: rotate(5deg); }
            100% { top: 65%; opacity: 0; transform: rotate(0deg); }
          }

          @keyframes flag-wave {
            0%, 100% { transform: rotate(0deg); }
            25%      { transform: rotate(-10deg); }
            75%      { transform: rotate(5deg); }
          }
  - type: custom:mushroom-chips-card
    chips:
      - type: template
        entity: input_boolean.mail_arrived
        icon: |-
          {% if states(entity) in ['on'] %}
            mdi:bell
          {% else %} 
            mdi:bell-cancel
          {% endif %}
        content: |-
          {% if states(entity) in ['on'] %}
            ARRIVED AT {{ as_timestamp(states[entity].last_changed) | timestamp_custom('%H:%M') }}
          {% else %} 
            NO MAIL
          {% endif %}
    alignment: end
    card_mod:
      style: >
        ha-card {
          /* ================================ */
          /* USER CONFIG HERE                 */
          /* ================================ */

          /* 1. ACTIVE STATES */
          {% set active_states = ['on'] %}
          
          /* 2. POSITIONING */
          {% set pos_y = 'top' %}               
          {% set pos_y_val = '30%' %}
          {% set pos_x = 'right' %}             
          {% set pos_x_val = '1%' %}

          /* 3. SIZING */
          {% set icon_size = '20px' %}          
          {% set text_size = '12px' %}          
          {% set badge_height = '5px' %}       
          {% set border_radius = '10px' %}    
          {% set border_thickness = '0.9px' %}
          
          /* 4. COLORS (HEX, RGB, Color Name) */
          /* --- Active --- */
          {% set active_bg = '#ffbc2c' %} 
          {% set active_icon = 'orange' %}
          {% set active_text = '#ffffff' %}
          {% set active_border = 'orange' %}
          {% set active_opacity = 0.3 %}
          
          /* --- Inactive --- */
          {% set inactive_bg = 'grey' %}
          {% set inactive_icon = 'grey' %}
          {% set inactive_text = 'white' %}
          {% set inactive_border = 'transparent' %}
          {% set inactive_opacity = 0.3 %}
          

          /* 5. ANIMATIONS */
          /* Options: pulse, shake, wobble   */
          /* flash, heartbeat, swing, vibrate  */
          
          {% set anim_type = 'shake' %}

          /* true = 24/7, false = only when active */
          {% set always_animate = false %}
          
          {% set anim_intensity_sec = 0.5 %} /* 0.1 to 1  */
          {% set anim_duration_sec = 1 %}
          {% set anim_delay_sec = 1 %}

          /* ================================== */
          /* USER CONFIG END (DONT TOUCH BELOW) */
          /* ================================== */

          {% set current_state = states(config.chips[0].entity) %}
          {% set is_active = current_state in active_states %}
          
          {% if is_active %}
            {% set current_bg = active_bg %}
            {% set current_icon = active_icon %}
            {% set current_text = active_text %}
            {% set current_border = active_border %}
            {% set current_op = active_opacity %}
            {% set current_anim = anim_type %}
          {% else %}
            {% set current_bg = inactive_bg %}
            {% set current_icon = inactive_icon %}
            {% set current_text = inactive_text %}
            {% set current_border = inactive_border %}
            {% set current_op = inactive_opacity %}
            {% set current_anim = anim_type if always_animate else 'none' %}
          {% endif %}

          {% set master_cycle = anim_duration_sec + anim_delay_sec %}
          {% set iterations = (anim_duration_sec / anim_intensity_sec) | round(0, 'floor') | int if anim_intensity_sec > 0 else 0 %}
          {% set step_percent = (anim_intensity_sec / master_cycle) * 100 if master_cycle > 0 else 0 %}

          --chip-padding: {{ badge_height }};
          --chip-border-radius: {{ border_radius }};
          
          --chip-border-width: 0px !important;
          --ha-card-border-width: 0px !important;
          
          --chip-box-shadow: 0 0 0 {{ border_thickness }} {{ current_border }} !important;

          --text-color: {{ current_icon }} !important;
          --color: {{ current_icon }} !important; 
          --chip-background: color-mix(in srgb, {{ current_bg }}, transparent {{ (1 - current_op) * 100 }}%) !important;
          --chip-font-size: {{ text_size }} !important;
          --chip-icon-size: {{ icon_size }} !important; 
          --mdc-icon-size: {{ icon_size }} !important;   
          --chip-height: auto !important;
          --primary-text-color: {{ current_text }} !important;
          --secondary-text-color: {{ current_text }} !important;

          position: absolute !important;
          {{ pos_y }}: {{ pos_y_val }};
          {{ pos_x }}: {{ pos_x_val }};
          z-index: 1;
          border: none !important;
          border-radius: {{ border_radius }} !important;
          box-shadow: 0 4px 6px rgba(0,0,0,0.2);
          overflow: visible !important;
          will-change: transform; 
          
          {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}
            animation: anim-{{ config.chips[0].entity | replace('.', '-') }}-{{ anim_type }} {{ master_cycle }}s infinite linear;
          {% endif %}
        }


        ha-state-icon, ha-icon {
          display: flex;
          align-items: center;
          justify-content: center;
          width: {{ icon_size }} !important; 
          height: {{ icon_size }} !important;
        }


        {% if config.chips[0].content is not defined or config.chips[0].content
        | length == 0 %}
           ha-card { padding: 5px !important; border-radius: 50% !important; aspect-ratio: 1/1; justify-content: center; }
           .content { display: none; }
        {% else %}
           ha-card { padding: 0 10px !important; }
           .content { padding-left: 5px; font-weight: bold; }
        {% endif %}


        {% if current_anim != 'none' and iterations > 0 and master_cycle > 0 %}

        @keyframes anim-{{ config.chips[0].entity | replace('.', '-') }}-{{
        anim_type }} {
          {% for i in range(iterations) %}
            {% set t = i * step_percent %} 
            
            {% if anim_type == 'pulse' %}
              {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.5) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

            {% elif anim_type == 'shake' %}
              {{ t + 0 }}% { transform: rotateZ(0deg); }
              {{ t + (step_percent * 0.25) }}% { transform: rotateZ(-10deg); }
              {{ t + (step_percent * 0.75) }}% { transform: rotateZ(10deg); }
              {{ t + step_percent }}% { transform: rotateZ(0deg); }

            {% elif anim_type == 'flash' %}
              {{ t + 0 }}% { opacity: 1; }
              {{ t + (step_percent * 0.5) }}% { opacity: 0; }
              {{ t + step_percent }}% { opacity: 1; }

            {% elif anim_type == 'wobble' %}
              {{ t + 0 }}% { transform: translate3d(0%, 0, 0); }
              {{ t + (step_percent * 0.3) }}% { transform: translate3d(-15%, 0, 0) rotateZ(-5deg); }
              {{ t + (step_percent * 0.6) }}% { transform: translate3d(10%, 0, 0) rotateZ(3deg); }
              {{ t + step_percent }}% { transform: translate3d(0%, 0, 0); }

            {% elif anim_type == 'heartbeat' %}
              {{ t + 0 }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.25) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + (step_percent * 0.5) }}% { transform: scale3d(1, 1, 1); }
              {{ t + (step_percent * 0.75) }}% { transform: scale3d(1.15, 1.15, 1); }
              {{ t + step_percent }}% { transform: scale3d(1, 1, 1); }

            {% elif anim_type == 'swing' %}
              {{ t + 0 }}% { transform: rotateZ(0deg); }
              {{ t + (step_percent * 0.4) }}% { transform: rotateZ(15deg); }
              {{ t + (step_percent * 0.8) }}% { transform: rotateZ(-10deg); }
              {{ t + step_percent }}% { transform: rotateZ(0deg); }

            {% elif anim_type == 'vibrate' %}
              {{ t + 0 }}% { transform: translate3d(0, 0, 0); }
              {{ t + (step_percent * 0.2) }}% { transform: translate3d(-2px, 2px, 0); }
              {{ t + (step_percent * 0.4) }}% { transform: translate3d(-2px, -2px, 0); }
              {{ t + (step_percent * 0.6) }}% { transform: translate3d(2px, 2px, 0); }
              {{ t + (step_percent * 0.8) }}% { transform: translate3d(2px, -2px, 0); }
              {{ t + step_percent }}% { transform: translate3d(0, 0, 0); }

            {% endif %}
          {% endfor %}
        }

        {% endif %}

```
</details>

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
