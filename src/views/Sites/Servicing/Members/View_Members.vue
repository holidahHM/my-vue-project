<template>
    <base-header class="pb-6 pb-8 pt-5 pt-md-8 bg-gradient-success">
    <div id="app">
	<div class="text-uppercase text-bold">id selected: {{selected}}</div>
	<table class="table table-striped table-hover">
		<thead>
			<tr>
				<th>
					<label class="form-checkbox">
    <input type="checkbox" v-model="selectAll" @click="select">
    <i class="form-icon"></i>
  </label>
				</th>
				<th>id</th>
				<th>name</th>
				<th>email</th>
			</tr>
		</thead>
		<tbody>
			<tr v-for="i in items">
				<td>
					<label class="form-checkbox">
    					<input type="checkbox" :value="i.id" v-model="selected">
						<i class="form-icon"></i>
  					</label>
				</td>
				<td>{{i.id}}</td>
				<td>{{i.name}}</td>
				<td>{{i.email}}</td>
			</tr>
		</tbody>
	</table>
</div>
    </base-header>
</template>
<script>
export default {
    data: () => ({
		items: [
			{
				id: "id1",
				name: "John Doe",
				email: "email@example.com"
			},
			{
				id: "id2",
				name: "Jone Doe",
				email: "email2@example.com"
			}
		],
		selected: [],
		selectAll: false
	}),
	methods: {
		select() {
			this.selected = [];
			if (!this.selectAll) {
				for (let i in this.items) {
					this.selected.push(this.items[i].id);
				}
			}
		}
	}
}
</script>