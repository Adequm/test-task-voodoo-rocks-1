<template>
	<div class="card">
	  <div class="card-body">
	    <h5 class="card-title">
	    	<a 
	    		href="#" 
	    		class="text-decoration-none" 
	    		v-text="title"
	    	/>
	    </h5>
	    <p class="card-text" v-text="body"/>
	    <p 
	    	style="cursor: pointer;"
	    	class="card-subtitle text-muted"
	    	:title="userNameTitle"
	    	v-text="userName"
	    	@click="$emit('clickToUserName', userName)"
	    />
	  </div>
	</div>
</template>

<script>

export default {
	name: 'AppCard',

	props: {
		title: String,
		body: String,
		userName: String,
		userNameTitle: {
			type: String,
			default: () => 'Поиск по имени пользователя'
		}
	}
};
</script>