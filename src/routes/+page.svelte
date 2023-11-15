<script lang="ts">
	import data from './data.json';
	let q: string = '';
	let a: string = '';
	let score: number = 0;
	let total: number = 0;
	let result: string = '';
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
	const choose = (choice: string) => {
		if (a === choice) {
			finish(true);
		} else {
			finish(false);
		}
		loadQuestion();
	}
	const finish = (scored: boolean) => {
		if (scored) {
			score++;
			result = `Correct! <b>${q}</b> is a <b>${a=="D" ? "drug" : "planet"}</b>.`;
		} else {
			result = `Incorrect! <b>${q}</b> is a <b>${a=="D" ? "drug" : "planet"}</b>.`;
		}
		total++;
		loadQuestion();
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
					{@html result}
				</ion-col>
			</ion-row>
			<ion-row>
				<ion-col>
					{score} / {total}
				</ion-col>
			</ion-row>
			<ion-row>
				<ion-col size={"5"}>
					<ion-button color="medium" expand="block" on:click={()=>{choose('D')}}>Drug</ion-button>
				</ion-col>
				<ion-col size={"2"} class="ion-text-center">
					<ion-button color="dark" fill="clear" disabled={true} expand="block">or</ion-button>
				</ion-col>
				<ion-col size={"5"}>
					<ion-button color="medium" expand="block" on:click={()=>{choose('P')}}>Planet</ion-button>
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