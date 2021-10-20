# Directus Vue Components

Directus comes shipped with its own Vue component library you can use to enrich your extensions, while making them fit
within the context of the Directus app.

These components can be used in any of the "front-end extensions", including [Interfaces](/guides/interfaces/),
[Displays](/guides/displays/), and [Modules](/guides/modules/).

### Breaking Changes

While we aim to make the components as backwards compatible as possible, we **do not** consider internal-API changes in
the components as a breaking change of Directus. Please keep an eye
[the release notes](https://github.com/directus/directus/releases), and make sure your extension still works before
updating Directus.

# Transition Bounce

Bounces items in or out depending if the get added or removed from the view.

```html
<template>
	<div>
		<v-button @click="toggle">Click me!</v-button>

		<transition-bounce>
			<div v-if="active">More content</div>
		</transition-bounce>
	</div>
</template>

<script lang="ts">
	import { defineComponent, ref } from 'vue';

	export default defineComponent({
		setup(props) {
			const active = ref<boolean>(false);

			return { active, toggle };

			function toggle() {
				active.value = !active.value;
			}
		},
	});
</script>
```

## Reference

#### Slots

| Slot      | Description                                  | Data |
| --------- | -------------------------------------------- | ---- |
| _default_ | The elements that should get animated in/out |      |
