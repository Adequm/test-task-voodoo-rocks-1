<template>
	<div class="wrapper">
		<i 
			:class="[`bi bi-${icon}`, { rotate }]"
			:style="{ fontSize: `${size}px` }"
		/>
		<span v-if="text" v-text="text"/>
	</div>
</template>

<script>

export default {
	name: 'AppLoader',

	props: {
		size: {
			type: Number,
			default: 100,
		},
		rotate: Boolean,
		text: String,
		icon: String
	},
}
</script>

<style lang="scss" scoped>
.wrapper {
  grid-column-start: 1;
  grid-column-end: 10;
  margin: auto;
  color: var(--app-loader-color);
  display: flex;
  flex-direction: column;
  text-align: center;
  i {
  	line-height: 1;
  	&.rotate::before {
	  	animation: rotate 2s linear infinite;
	  }
  }
}

@keyframes rotate {
  to { transform: rotate(360deg) }
}
</style>