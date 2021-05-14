<template>
	<div id="app" class="container-fluid py-4" >
		<div class="container-fluid">
			<div class="">
				<h2>
					Caso de uso 1, Funil de vendas.
				</h2>

				<hr>
			</div>
			<div class="row">
				<div class="col-md-4">
					<div class="container">
						<h4>
							Status 1
						</h4>
						<draggable 
							class="dragArea list-group"
							v-model="status1" 
							:group="{ name: 'first_name'}" 
						>
							<div v-for="element in status1" :key="element.id" class="list-group-item" >
								{{element.first_name}}
							</div>
						</draggable>
					</div>
				</div>
				<div class="col-md-4">
					<div class="container">
						<h4>
							Status 2
						</h4>
						<draggable 
							class="dragArea list-group"
							v-model="status2" 
							:group="{ name: 'first_name'}" 
						>
							<div v-for="element in status2" :key="element.id" class="list-group-item" >
								{{element.first_name}}
							</div>
						</draggable>
					</div>
				</div>

				<div class="col-md-4">
					<div class="container">
						<h4>
							Status 3
						</h4>
						<draggable 
							class="dragArea list-group"
							v-model="status3" 
							:group="{ name: 'first_name'}" 
						>
							<div v-for="element in status3" :key="element.id" class="list-group-item" >
								{{element.first_name}}
							</div>
						</draggable>
					</div>
				</div>
			</div>

			<hr class="my-4">

			<div class="container">
				<h2>Json resultante</h2>

				{{allSelectedStatus}}
			</div>
		</div>

		<div class="container-fluid py-4 mt-4">
			<div>
				<h2>Caso 2, Leads</h2>

				<hr>

				<a href="#" class="btn btn-primary" @click.prevent="addColumnsToShedules" >
					Adicionar horário
				</a>
			</div>

			<div class="row">
				<div class="col-3">
					<div class="container">
						<h3 class="text-center" >Horários</h3>
						<ul class="list-group" >
							<li
								v-for="(e, id) in shedules"
								:key="id + '-shedules-time'"
								@click.prevent="removeFromShedules(id)"
								class="list-group-item"
							>
								{{e.start + ':30 as ' + e.stop + ':30'}}
							</li>
						</ul>
					</div>
				</div>
				<div class="col-3">
					<div class="container">
						<h3 class="text-center">Segunda</h3>

						<draggable 
							v-for="(e, id) in shedules"
							:key="id + '-shedules'"
							class="dragArea list-group"
							v-model="shedules[id].days.monday" 
							:group="{ name: 'first_name'}" 
						>
							<div v-for="(element,i) in shedules[id].days.monday" :key="element.id + '-e-' + i" class="list-group-item" @click.prevent="removeLineFromShedules(shedules[id].days.monday, i)" >
								{{element.first_name}}
							</div>

							<hr class="my-2" >
						</draggable>
					</div>
				</div>
				<div class="col-3">
					<div class="container">
						<h3 class="text-center">Terça</h3>

						<draggable 
							v-for="(e, id) in shedules"
							:key="id + '-shedules'"
							class="dragArea list-group"
							v-model="shedules[id].days.tuesday" 
							:group="{ name: 'first_name'}" 
						>
							<div v-for="(element, i) in shedules[id].days.tuesday" :key="element.id+ '-e-' + i" class="list-group-item"  @click.prevent="removeLineFromShedules(shedules[id].days.tuesday, i)" >
								{{element.first_name}}
							</div>

							<hr class="my-2" >
						</draggable>
					</div>
				</div>

				<div class="col-2">
					<div class="container">
						<h3>Usuários disponiveis</h3>
						<draggable 
							class="dragArea list-group"
							v-model="users_shedules" 
							:group="{ name: 'first_name', pull: 'clone', put: false}" 
						>
							<div v-for="element in users_shedules" :key="element.id" class="list-group-item" >
								{{element.first_name}}
							</div>
						</draggable>
					</div>
				</div>
			</div>

			<hr class="my-4">

			<div class="container">
				<h2>Json resultante</h2>

				{{shedules}}
			</div>
		</div>
	</div>
</template>

<script>
  	import draggable from 'vuedraggable'
	export default {
		name: "App",
		components: {
			draggable
		},
		data(){
			return {
				status1: [{
					id: 1,
					first_name: 'First Person',
					title: 'Corretor'
				}, {
					id: 2,
					first_name: 'Second Person',
					title: 'Corretor'
				},{
					id: 3,
					first_name: 'Third Person',
					title: 'Administrador'
				}],

				status2: [],
				status3: [],

				shedules: [],

				users_shedules: [{
					id: 1,
					first_name: 'First Person',
					title: 'Corretor'
				}, {
					id: 2,
					first_name: 'Second Person',
					title: 'Corretor'
				},{
					id: 3,
					first_name: 'Third Person',
					title: 'Administrador'
				}]
			}
		},

		computed: {
			allSelectedStatus(){
				return {
					status1: this.status1,
					status2: this.status2,
					status3: this.status3
				}
			}
		},

		methods: {
			addColumnsToShedules(){
				let start = 0
				let stop = 0

				if (this.shedules.length == 0){
					start = 8
					stop  = start + 2
				}else{
					start = this.shedules[this.shedules.length - 1].stop
					stop  = start + 2
				}

				this.shedules.push({
					start: start,
					stop: stop,
					days: {
						monday: [],
						tuesday: [],
						wednesday: [],
						thursday: [],
						friday: [],
						saturday: [],
						sunday: []
					}
				})
			},

			removeLineFromShedules(shedule, id){
				shedule.splice(id, 1)
			},

			removeFromShedules(id){
				this.shedules.splice(id, 1)
			}
		}
	};
</script>

<style>
</style>
