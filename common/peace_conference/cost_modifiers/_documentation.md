# Scripted cost modifiers for peace actions

## How to access data from the peace action
- `ROOT` - Negotiator country (who is doing the negotiation)
- `FROM` - Taker country (who will be the owner after the conference)
- `FROM.FROM` - Giver country (who was the owner before the conference)
- `FROM.FROM.FROM` - State (iff peace action refers to a state)

You might have to use e.g. `ROOT.FROM` to access the variable from inside another scope

## Possible peace action types
- `take_states`
- `puppet`
- `force_government`
- `liberate`

```
peace_action_modifiers = {
	# All cost modifiers that are active for a peace action are multiplied together. Try to use values close to 1.0 (maybe around [0.4 - 3.0]). Extreme values seldom work as well as you think.

	arbitrary_name_for_the_cost_modifier = {

		category = is_core  # Category is used for grouping modifiers in e.g. tooltips. All modifiers with the same category will be shown as just one number. No specified category ends up under "other".

		peace_action_type = force_government  # For which action types should the cost modifier be active. Can take a single type or an array of types, e.g. { puppet liberate }

		enable = {  # Determines when this modifier should be active for a peace action. Can contain any trigger, including the peace conference-specific ones.
			# Note that some things (e.g. diplomatic relations, state ownership, etc) are not updated during a peace conference but instead before or after. Use the peace conference-specific triggers (trigger names starting with pc_xxx, e.g. pc_is_puppeted_by) to check for things that happen during a conference.

			ROOT = { has_government = democratic }  # checks if negotiator is democratic

			ROOT.FROM = { tag = AUS }  # Checks if the taker is Austria

			ROOT.FROM.FROM.FROM = { is_core_of = AUS }  # Checks if the state targeted by the peace action is a core of Austria

		}

		cost_multiplier = 0.65  # Multiply the action cost by 0.65, i.e. reduce cost by 35%. Must be > 0.

	}
}
```