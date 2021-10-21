<template>
	<div id="app">
		<h1 class="bg-primary text-white text-center p-2">
			List tareas {{ user }}
		</h1>

		<div class="container-fluid p-4">

      <div class="row" v-if="filtroTareas.length === 0">
        <div class="col text-center">
          <b>bravo has completado todas tus tareas</b>
        </div>
      </div>

      <template v-else>
			<div class="row">
				<div class="col font-weight">Tarea</div>
				<div class="col2 font-weight-bold">Terminada</div>
			</div>
			<div
				class="row"
				v-for="tarea in filtroTareas"
				v-bind:key="tarea.description"
			>
				<div class="col">{{ tarea.description }}</div>
				<div class="col-2 text-center">
					<input
						type="checkbox"
						v-model="tarea.terminada"
						class="form-check-input"
					/>
					{{ tarea.terminada }}
				</div>
			</div>
      </template>
			<div class="row py-2">
				<div class="col">
					<input class="from-control" v-model="newItem" />
				</div>
				<div class="col-2">
					<button v-on:click="addTarea" class="btn btn-primary">Agregar</button>
				</div>
			</div>

			<div class="row bg-dark py-2 mt-2 text-white">
				<div class="col text-center">
					<input
						type="checkbox"
						class="form-check-input"
						v-model="banderaCompletada"
					/>
					<label class="form-check-label font-weight-bold">
						ocultar las tareas completadas
					</label>
				</div>
        <div class="col text-center">
          <button v-on:click=" eliminarTarea" class="btn btn-sm btn-warning">Eliminar Tarea completa</button>
    
        </div>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				user: "Thierno",
				newItem: "",
				tareas: [],
				banderaCompletada: true,
			};
		},
		computed: {
			filtroTareas() {
				return this.banderaCompletada
					? this.tareas.filter((tarea) => !tarea.terminada)
					: this.tareas;
			},
		},
		methods: {
			addTarea() {
				this.tareas.push({
					description: this.newItem,
					terminada: false,
				});
     this.guardarTarea(),
				this.newItem = "";
			},
      guardarTarea(){
           localStorage.setItem('tareas',JSON.stringify(this.tareas))
      },
      eliminarTarea(){
        this.tareas = this.tareas.filter(tarea => !tarea.terminada)
        this.guardarTarea()
      }

		},
    created(){
      let data = localStorage.getItem("tareas");
      if(data !=null){
        this.tareas = JSON.parse(data)
      }
    },
	};
</script>
