<script lang="ts">
	import data from './data.json';
	let q: string = '';
	let a: string = '';
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
	}
	loadQuestion();
</script>
<ion-card>
	<ion-card-header>
		<div class="header">Is it a drug, or is it a planet?</div>
	</ion-card-header>
	<ion-card-title class="ion-padding"><ion-label>{q}</ion-label></ion-card-title>
	<!-- <ion-card-subtitle class="ion-padding"><ion-label>Subtitle</ion-label></ion-card-subtitle> -->
	<ion-card-content>
		<ion-grid>
			<ion-row>
				<ion-col>
					<ion-button color="danger" expand="block" on:click={loadQuestion}>Drug</ion-button>
				</ion-col>
				<ion-col>
					<ion-button color="success" expand="block" on:click={loadQuestion}>Planet</ion-button>
				</ion-col>
			</ion-row>
		</ion-grid>
	</ion-card-content>
</ion-card>
<style>
	.header {
		font-size: larger;
		color: darkblue;
	}
</style>