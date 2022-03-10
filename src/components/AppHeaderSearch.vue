<template>
  <form @submit.prevent="$emit('submit')">
    <div class="form-group">
      <div class="input-group mb-3">
        <div class="input-group-prepend" v-if="iconPrepend">
          <span 
          	class="input-group-text" 
          	style="border-radius: 0.25rem 0 0 0.25rem; cursor: pointer;"
          	@click="$emit('submit')"
          	id="basic-addon1">
            <i :class="`bi bi-${iconPrepend}`"/>
          </span>
        </div>
        <input 
          type="text" 
          class="form-control" 
          :placeholder="placeholder" 
          :value="value"
          aria-label="basic"
          aria-describedby="basic-addon1"
          @input="$emit('input', $event.target.value)"
        >
        <div class="input-group-append" v-if="iconAppend">
          <span 
            class="input-group-text" 
            style="border-radius: 0 0.25rem 0.25rem 0; cursor: pointer;"
            @click="value && $emit('input', '') && $emit('submit')"
            id="basic-addon1">
            <i :class="`bi bi-${iconAppend}`"/>
          </span>
        </div>
      </div>
    </div>
  </form> 
</template>

<script>

export default {
	name: 'AppHeaderSearch',

	props: {
		value: String,
		iconPrepend: {
			type: String,
			default: () => 'search',
		},
    iconAppend: {
      type: String,
      default: () => 'trash',
    },
		placeholder: {
			type: String,
			default: () => 'Имя пользователя',
		},
	},
};
</script>