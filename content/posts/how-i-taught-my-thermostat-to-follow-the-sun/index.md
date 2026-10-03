---
title: How I Taught My Thermostat to Follow the Sun
date: 2025-11-08T16:20:00
draft: true
description: A story about how I automated my home heating with Home Assistant to react dynamically to solar power production, turning excess sunlight into comfort, not wasted watts.
tags:
  - homeassistant
  - automation
  - solar
  - energy
  - smart-home
  - homelab
cover: null
---

### The Day I Realized My Solar Panels Were Heating the Wrong House

It was a Tuesday afternoon in October. Sunny, cold, and I was sitting at my desk watching my Home Assistant dashboard when it hit me: my solar panels were producing 3.2kW of power, my house was consuming maybe 800W, and the rest (about 2.4kW) was flowing back to the grid.

Meanwhile, my heat pump was happily consuming grid electricity to keep my thermostats at their scheduled 23.5°C.

### The moment it hit me

The idea came to me out of nowhere. I already had something perfect for storing energy: two floors of house with concrete, brick, and plaster that could act as a giant thermal battery.

What if I just… turned up the thermostats when the sun was shining?

The concept was simple: boost the heating when solar production is high, store that energy in the building’s thermal mass, and coast through the evening on stored heat. Free heating, no new hardware, just some clever automation.

### Starting with the right logic

I knew from the beginning that this needed to be smart. Romanian weather is unpredictable; clouds pass, production fluctuates, and I didn’t want the thermostats bouncing around every few minutes, especially with a heat pump in the mix.

The solution was clear: don’t treat solar production like a simple on/off switch.

Adding intelligence: **the timer solution**.

I needed to distinguish between *“cloudy period”* and *“random cloud passing by.”*

I added timers.

**For boost activation**: wait 15 minutes of sustained high production before changing anything. This filters out brief spikes and only activates on genuinely sunny periods.

**For boost deactivation**: wait 30 minutes before dropping back to normal. Why longer? Because once you’ve heated the thermal mass, you want to keep it there as long as possible. The stored heat is valuable.

Then I added hysteresis. Boost activates at 2.5kW but only deactivates below 2.0kW. This creates a “comfort zone” where the system isn’t constantly second-guessing itself.

The result? Smooth, predictable operation that responds to real weather patterns, not momentary fluctuations.

### The morning problem

About two days in, I discovered a new issue during my morning coffee. At 6 AM, the upstairs thermostat shifted to its daytime setting, effectively cooling the room when it still needed to stay comfortable.

The downstairs felt fine, we were moving around, making breakfast, getting ready. But upstairs, where people were still sleeping until 8 or 9? It got noticeably cooler right when you wanted it warmest.

I needed different morning schedules for each floor.

The solution was simple once I figured it out: at 6 AM, only the downstairs thermostat switches to day mode. Upstairs stays at the cozy 24°C until 9 AM, when it finally drops to 23.5°C. Three hours of morning comfort that costs almost nothing because the heat pump is running anyway.

### When automation meets reality

After a few days, I tried adjusting the downstairs thermostat manually because the room was already warmer than it needed to be. A few minutes later, the automation decided it knew better. When Solar Boost kicked in, it overrode my change and pushed the temperature right back up.

That was the moment I realized the system needed boundaries.

I added override flags. Three of them, actually:

- One for upstairs manual changes
- One for downstairs manual changes
- One global “I’m taking control” flag

The key insight was when to reset these overrides. Not immediately, that would be annoying. But not never, or the automation becomes useless.

The compromise: reset all overrides at major mode transitions (6 AM morning mode, 8 PM night mode). These are times when the house schedule is changing anyway, so it feels natural for the automation to reassert itself.

The system now respects human intervention but doesn’t permanently defer to it.

### Making it visible

One problem with home automation is the “is it working?” question. You don’t want to walk over to check the thermostat every hour, but you want to know what the system is doing.

I created a status sensor that tells the story:

```text
Day Normal (PV: 1.8kW)
Activating in 8:23…
Boost Active (PV: 3.1kW)
Deactivating in 24:15…
Night Mode Active
```

Now I can glance at my phone and instantly understand the system state.

### A Typical Sunny Day

By mid-October, the system had settled into a rhythm I could watch like a dance:

- 6:00 AM - the downstairs thermostat comes alive, bringing the living room to 23.5°C. Upstairs stays at 24°C.
- 9:00 AM - upstairs finally drops to 23.5°C. Everyone’s awake now anyway.
- 10:30 AM - solar production crosses 2.5kW as the sun climbs higher. Timer starts.
- 10:45 AM - boost mode activates. Both thermostats jump to 24.5°C. The heat pump kicks in, pushing heat into the walls and floors.
- 11:00 AM - 5:00 PM - the house hovers at that elevated temperature, soaking up free solar energy. The concrete slab under my floors is literally charging like a battery.
- 4:30 PM - clouds roll in, production drops to 1.8kW. Deactivation timer starts.
- 5:00 PM - system returns to 23.5°C. The heat pump eases off, but the house stays comfortable. All that absorbed heat is slowly radiating back.
- 7:00 PM - outside temperature drops, but we’re still coasting on stored solar heat. The heat pump barely runs.
- 8:00 PM - night mode. Thermostats adjust for sleeping temperatures. The electricity meter has barely moved all day.

### The Numbers

I don’t have perfect before/after comparisons (I changed too many variables at once), but the patterns are clear:

On sunny days, the heat pump runs from 10:45-4:00 PM when energy is free, then barely runs from 5:00-10:00 PM. The thermal mass bridges that gap.

The real win isn’t just the energy saved, it’s the timing. The house consumes energy when the sun is shining and my panels are producing, then coasts through the evening on stored heat.

### The takeaways from building it

This project reinforced something I try to teach my team: good automation isn’t about eliminating human control, it’s about handling the boring parts so humans can focus on the exceptions.

The automation handles the daily dance between sun and clouds. But when someone’s genuinely cold and turns up the heat? It backs off. When morning routines change on weekends? Manual override. The system is assertive but not stubborn.

### The Future (Maybe)

This system already feels alive, but there’s always room to evolve. The next steps are about giving it more context and intelligence and letting it anticipate rather than just react.

- Hook into weather forecasts to pre-heat more aggressively before cloudy periods.
- Adjust the heat pump outlet temperature dynamically based on outside temperature and humidity.
- Add a “summer mode” that activates when the battery is full, using excess solar energy for thermal storage instead of exporting it.
- Integrate battery state of charge to decide when Solar Boost should trigger or hold back.
- Log and visualize heat retention curves to fine-tune insulation and circulation efficiency.
- Trigger alerts for abnormal energy use when heating demand doesn’t align with solar conditions.

### What this really means

There’s something deeply satisfying about building systems that just work. Not flashy, not complicated for the sake of complexity, just elegantly solving a real problem. This automation runs quietly in the background, making hundreds of small decisions every day, and the house is warmer, the bills are lower, and nobody has to think about it.

That’s what good engineering looks like. It’s not about the cleverest code or the most complex architecture. It’s about understanding a problem deeply enough to build something that fades into the background and simply does its job.
