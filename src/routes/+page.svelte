<script lang="ts">
	import { onMount } from 'svelte'
	import data from './data.json';
	//import { modalController } from '@ionic/core'
	import { modalController } from 'ionic-svelte'
	import { page } from '$app/stores'

	import endgame from './endgame.svelte';
	const QUESTION_COUNT: number = parseInt($page.url.searchParams.get('count') || '20');
	const QUESTION_DELAY: number = parseInt($page.url.searchParams.get('delay') || '1500');
	let q: string = '';
	let a: string = '';
	let score: number = 0;
	let total: number = 0;
	let result: string = '';
	let buttonsActive = false;

	const getQuestion = () => {
		const index = Math.floor(Math.random() * data.length);
		const q = data[index];
		data.splice(index, 1);
		return q;
	}
	const loadQuestion = () => {
		const qq: any = getQuestion();
		const arr = qq.split('|');
		q = arr[0];
		a = arr[1];
		buttonsActive = true;
	}
	const choose = (choice: string) => {
		if (buttonsActive) {
			buttonsActive = false;
			if (a === choice) {
				finish(true, choice);
			} else {
				finish(false, choice);
			}
		}
	}
	const finish = async (scored: boolean, choice: string) => {
		const el: any = document.getElementById(choice=='D' ? 'drugbutton' : 'planetbutton');
		if (scored) {
			el.color = 'success';
			score++;
			result = `Correct! <b>${q}</b> is a <b>${a=="D" ? "drug" : "planet"}</b>.`;
		} else {
			el.color = 'danger';
			result = `Incorrect! <b>${q}</b> is a <b>${a=="D" ? "drug" : "planet"}</b>.`;
		}
		total++;
		setTimeout(async () => {
			el.color = 'light';
			result = '';
			if (total >= QUESTION_COUNT) {
				const {data,error} = await openModal(endgame,{total,score});
				total = 0;
				score = 0;
			}
			loadQuestion();
		}, QUESTION_DELAY);
	}
	onMount(() => {
		loadQuestion();
	});
	const openModal = async (theModal: any, theProps: any = {}, theOptions: any = {}) => {
		const obj: any = {
			component: theModal,
			componentProps: theProps,
			showBackdrop: true,
			backdropDismiss: true,
		};
		for (const key in theOptions) {
			obj[key] = theOptions[key];
		}
		const openModalController = await modalController.create(obj)
		openModalController.present();
		setTimeout(() => {resizeModal(openModalController)}, 200);
		const { data } = await openModalController.onWillDismiss();
		return data;
	}
	const resizeModal = async (openLoginModalController: HTMLIonModalElement) => {
		setTimeout(() => {
			const pg = openLoginModalController.getElementsByClassName('ion-page')[0];
			const header = pg.getElementsByTagName('ion-header')[0];
			const content = pg.getElementsByTagName('ion-content')[0];
			const footer = pg.getElementsByTagName('ion-footer')[0];
			let p = pg.clientHeight;
			if (header) p -= header.clientHeight;
			if (footer) p -= footer.clientHeight;
			content.style.height = p + 'px';
		}, 200);
	}	
</script>
<ion-card>
	<!-- <ion-card-header>
		<div class="header">Is it a drug, or is it a planet?</div>
	</ion-card-header> -->
	<ion-card-title color="light" class="ion-padding ion-text-center"><ion-label>{q}</ion-label></ion-card-title>
	<!-- <ion-card-subtitle class="ion-padding"><ion-label>Subtitle</ion-label></ion-card-subtitle> -->
	<ion-card-content>
		<ion-grid>
			<!-- <ion-row>
				<ion-col>
					{@html result}
				</ion-col>
			</ion-row> -->
			<ion-row>
				<ion-col size={"5"}>
					<ion-button id="drugbutton" color="light" expand="block" on:click={()=>{choose('D')}}>Drug</ion-button>
				</ion-col>
				<ion-col size={"2"} class="ion-text-center">
					<ion-card-title color="light" class="ion-text-center or-text"><ion-label>or</ion-label></ion-card-title>
					<!-- <ion-button color="light" fill="clear" disabled={true} expand="block"><b>or</b></ion-button> -->
				</ion-col>
				<ion-col size={"5"}>
					<ion-button id="planetbutton" color="light" expand="block" on:click={()=>{choose('P')}}>Planet</ion-button>
				</ion-col>				
			</ion-row>
			<ion-row>
				<ion-col class="ion-text-center">
					<ion-card-title color="light" class="ion-padding ion-text-center"><ion-label>{score} / {total}</ion-label></ion-card-title>					
				</ion-col>
			</ion-row>
		</ion-grid>
	</ion-card-content>
</ion-card>
<style>
	.header {
		font-size: larger;
	}
	.or-text {
		font-size: large;
		padding-top: 1em;
	}	
</style>