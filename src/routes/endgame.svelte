<script lang="ts">
	import { IonPage } from 'ionic-svelte'
	import { modalController } from '@ionic/core'
	// import { onMount } from "svelte"
	export let total = 0
	export let score = 0
	let title = 'Drug or Planet?'
	const closeOverlay = (data?: any, error?: any) => {
		modalController.dismiss({ data, error })
	}
</script>

<IonPage>
	<ion-header translucent={true}>
		<ion-toolbar id="selectorToolbar">
			<ion-title>{title}</ion-title>
		</ion-toolbar>
	</ion-header>
	<ion-content>
		<div id="selectorDiv" class="ion-padding">
			<div class="vertical-align">
				<div class="score-text">Game over.</div>
				<div class="score-text">You scored {score} out of {total}.</div>
				<div class="ion-padding">
					<ion-button color="medium" expand="block" on:click={closeOverlay}>Start Over</ion-button>
				</div>
			</div>
		</div>
	</ion-content>
</IonPage>

<style>
	.vertical-align {
		display: grid;
		place-items: center;
		height: 500px; /* Or the desired height */
	}
    .score-text {
        /* font-size: larger;
        font-weight: bold; */
    }
</style>
