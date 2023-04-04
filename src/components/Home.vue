<template>
	<Layout>
		<template #header>
			<Header/>
		</template>
		<template #resume>
			<Resume
				:label="label"
				:total-amount="totalAmount"
				:amount="amount"
			>
				<template #graphic>
					<Graphic
						:amounts="amounts"
						@select="select"
					/>
				</template>
				<template #action>
					<Action @create="create"/>
					<!--<Action/>-->
				</template>
			</Resume>
		</template>
		<template #movements>
			<Movements 
				:movements="movements"
				@remove="remove"
			/>
		</template>
	</Layout>
</template>

<script>
// option API

import Layout from "@/components/Layout.vue";
import Header from "@/components/Header.vue";
import Resume from "@/components/Resume/Index.vue";
import Action from "@/components/Action.vue";
import Movements from "@/components/Movements/Index.vue";
import Graphic from "@/components/Resume/Graphic.vue";

export default {
	components: {
		Layout,
		Header,
		Resume,
		Action,
		Movements,
		Graphic,
	},
	data() {
		return {
			amount: null,
			label: "Monto Total",
			// amounts: [100, 200, 500, 200, -400, -600, -300, 0, 300, 500],
			movements: [
			// {	id: 0,
			// 	title: "moviemiento 1",
			// 	description: "cualquier cosa esta bien",
			// 	amount: 100,
			// 	time: new Date("03-10-2023"),
			// },{
			// 	id: 1,
			// 	title: "moviemiento 2",
			// 	description: "cualquier cosa esta bien",
			// 	amount: 200,
			// 	time: new Date("03-10-2023"),
			// },{
			// 	id: 2,
			// 	title: "moviemiento 3",
			// 	description: "cualquier cosa esta bien",
			// 	amount: 500,
			// 	time: new Date("03-10-2023"),
			// },{
			// 	id: 3,
			// 	title: "moviemiento 4",
			// 	description: "cualquier cosa esta bien",
			// 	amount: 200,
			// 	time: new Date("03-10-2023"),
			// },{
			// 	id: 4,
			// 	title: "moviemiento 5",
			// 	description: "cualquier cosa esta bien",
			// 	amount: -400,
			// 	time: new Date("03-10-2023"),
			// },{
			// 	id: 5,
			// 	title: "moviemiento 6",
			// 	description: "cualquier cosa esta bien",
			// 	amount: -600,
			// 	time: new Date("03-10-2023"),
			// },{
			// 	id: 6,
			// 	title: "moviemiento 7",
			// 	description: "cualquier cosa esta bien",
			// 	amount: -300,
			// 	time: new Date("03-10-2023"),
			// },{
			// 	id: 7,
			// 	title: "moviemiento 8",
			// 	description: "cualquier cosa esta bien",
			// 	amount: 0,
			// 	time: new Date("03-10-2023"),
			// },{
			// 	id: 8,
			// 	title: "moviemiento 9",
			// 	description: "cualquier cosa esta bien",
			// 	amount: 300,
			// 	time: new Date("02-10-2023"),
			// },{
			// 	id: 9,
			// 	title: "moviemiento 10",
			// 	description: "cualquier cosa esta bien",
			// 	amount: 500,
			// 	time: new Date("02-10-2023"),}
			],
		}
	},
	computed: {
		amounts() {
			const lastDays = this.movements
			.filter(m => {
				const today = new Date();
				const oldDate = today.setDate(today.getDate() - 31);

				return m.time > oldDate;
			})
			.map(m => m.amount)

			return lastDays.map((m, i) => {
				const lastMovements = lastDays.slice(0 , i + 1);

				return lastMovements.reduce((suma, movement) => {
					return suma + movement;
				}, 0);
			});
		},
		totalAmount(){
			return this.movements.reduce((suma, m) => {
				return suma + m.amount;
			}, 0);
		}
	},
	mounted(){
		const oldMovements = JSON.parse(localStorage.getItem("movements")); //busco lo guardado antes
		//JSON.parse sirbe para recuperar la información a como la guarde abajo.
		if(Array.isArray(oldMovements)) { //confirma si la variable esta vacia.
			this.movements = oldMovements.map(m => {
			return { ...m , time: new Date(m.time)}; //con esto cambio las fechas al formato correcto.
			}); //lo pongo en la valiable movements para su uso.	
		}	
	},
	methods: {
		create(movement) {
			this.movements.push(movement);
			this.save();
		},
		remove(id) {
			const index = this.movements.findIndex(m => m.id === id);
			// console.log(id);
			this.movements.splice(index, 1);
			this.save();
		},
		save(){
			localStorage.setItem("movements", JSON.stringify(this.movements)); //con esta función de guarda en local. con JSON.stringify lo guarda como cadena de texto.
		},
		select(el){
			this.amount = el;
		},
	}
};

</script>