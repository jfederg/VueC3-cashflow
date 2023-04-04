<template>
	<header>
		<img
			@click="reload"
			src="@/assets/cashflow-icon.svg" 
			alt="Logo"
		/>
		<h1 @click="reload"><span>Cash</span>Flow</h1>
		<span></span>
	</header>
</template>

<script>
	// option API
	export default {
		methods: {
			reload(){
				location.reload();
				// console.log("Como que corre la función.");
			},
		},
	};
</script>

<style scoped>
	header {
	  display: flex;
	  justify-content: space-around;
	  align-items: center;
	  width: 100vw;
	}
	img,
	span {
	  width: 48px;
	}
	h1 {
	  font-size: 1.4rem;
	  color: var(--brand-blue);
	}
	h1 span {
	  color: var(--brand-green);
	}	
</style>