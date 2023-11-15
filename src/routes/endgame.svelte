<script lang="ts">	
    import { IonPage } from "ionic-svelte"
	import { modalController } from '@ionic/core'
	// import { onMount } from "svelte"
    export let total = 0;
    export let score = 0;
	let title = 'Game Over';
	const closeOverlay = (data?: any, error?: any) => {
		modalController.dismiss({ data, error });
	};
	
</script>
<IonPage>
<ion-header translucent={true}>
	<ion-toolbar id="selectorToolbar">
		<ion-title>{title}</ion-title>
	</ion-toolbar>
</ion-header>
<ion-content>
	<div id="selectorDiv" class="ion-padding">
        <div>You scored {score} out of {total}.</div>
	</div>
    <ion-button color="primary" expand="block" on:click={closeOverlay}>Start Over</ion-button>
</ion-content>
</IonPage>
<style>
</style>
