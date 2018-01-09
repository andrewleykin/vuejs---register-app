<template>
	<div>
		<form class="input-group-append mt-5">
			<input v-model="title" type="text" class="form-control" placeholder="Добавить сериал... ">
			<span class="input-group-btn">
				<button @click.prevent="addSerial" class="btn btn-secondary">Добавить!</button>
			</span>
		</form>
		<ol class="mt-5">
			<li v-for="(serial,index) in data.serials" :key="serial" @click="removeSerial(index)">
				{{ serial }}
			</li>
		</ol>
		<div class="mt-5">
			<button class="btn btn-primary" @click.prevent="chooseRandomSerial(0, data.serials.length)">Выбрать случайный сериал</button>
			<h2 class="mt-3 mb-3">{{ data.currentSerial }}</h2>
			<button class="btn btn-primary" @click.prevent="removeCurrentSerial" >Выбранный сериал просмотрен</button>
		</div>
	</div>
</template>

<script>
	export default {
		name: 'main-page',
		data() {
			return {
				title: '',
				data: {
					serials: [],
					currentSerial: 'Сериал не выбран...',
					numberOfCurrentSerial: null
				}
			}
		},
		props: ['uid'],
		methods: {
			addSerial(){
				if(this.title) {
					this.data.serials.push(this.title);
				}

				this.firebaseUpdate();

				this.title = '';
			},
			removeSerial(index){
				this.data.serials.splice(index, 1);

				if(this.data.numberOfCurrentSerial === index) {
					this.data.currentSerial = 'Сериал не выбран...';
					this.data.numberOfCurrentSerial = null;
				}

				for(var i=0; i < this.data.serials.length; i++) {
					if(this.data.currentSerial === this.data.serials[i]) {
						this.data.numberOfCurrentSerial = i;
					}
				}

				this.firebaseUpdate();
			},
			chooseRandomSerial(min, max) {
				const random = Math.floor(Math.random() * (max - min) + min);
				
				if(this.data.serials.length > 0) {
					this.data.currentSerial = this.data.serials[random];
					this.data.numberOfCurrentSerial = random;
				}

				this.firebaseUpdate();
			},
			removeCurrentSerial() {
				if(this.data.numberOfCurrentSerial !== null && this.data.numberOfCurrentSerial) {
					this.data.serials.splice(this.data.numberOfCurrentSerial, 1);
					this.data.currentSerial = 'Сериал не выбран...';
					this.data.numberOfCurrentSerial = null;
				}
				this.firebaseUpdate();
			},
			firebaseUpdate() {
				firebase.database().ref('users/' +this.uid).set({
					data: this.data
				});
			}
		},
		created(){
			this.data.serials = [];

			const takeSerials = firebase.database().ref('users/' + this.uid + '/data');

			takeSerials.on('value', (snapshot) => {
				if(snapshot.val().currentSerial && snapshot.val().serials) {
					this.data = snapshot.val();
				} else {
					this.data.serials = [];
					this.data.currentSerial = 'Сериал не выбран...';
				}
			});
		}
	}
</script>

<style>
	
</style>