<template>
	<div class="container-fluid">
		<div class="row">
			<main role="main" class="col-md-12 ml-sm-auto col-lg-12 pt-3 px-4">
				<div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pb-2 mb-3 ">
					<h2>VueSheet List</h2>
					<div class="btn-toolbar mb-2 mb-md-0">
						<a 	href="https://docs.google.com/spreadsheets/d/1oWHAFng2KoKBqYokzi4tO0hSDZD5pu0F8IIdMDRGssU/edit?usp=sharing"
							class="btn btn-sm btn-outline-secondary"
							target="_blank">
						View Google Sheet
						</a>
					</div>
				</div>
				<div class="table-responsive">
				<table class="table table-striped ">
					<thead>
					<tr>
						<th>Name</th>
						<th>Alamat</th>
						<th>Nama Penerima Bantuan</th>
						<th>Alamat Rekening (jika bantuan berupa dana tunai)</th>
            <th>Nama PIC yayasan/panti asuhan/gereja/perorangan dll</th>
            <th>Phone</th>
            <th>PIC Telkomsel (Corcom/PD)</th>
            <th>Jenis Kebutuhan
              (sembako, dana tunai, dll)</th>
            <th>Rp.</th>
            <th>Kategori</th>
					</tr>
					</thead>
					
					<tbody>
						<Row v-bind:key="row.id" v-for="row in rows" v-bind:row="row" />
					</tbody>
				</table>
				</div>
			</main>
		</div>
	</div>
</template>
 
<script>
import Row from '@/components/Row.vue';
const { GoogleSpreadsheet } = require('google-spreadsheet');
const creds = require('@/client_secret.json');
	export default {
		name: "Sheet",
		components: {
			Row
		},
		props: ["sheet"],
		data() {
			return {
				rows: [],
				loading: true,
			}
		},
		methods:{
			async accessSpreadSheet() {
				const doc = new GoogleSpreadsheet('1oWHAFng2KoKBqYokzi4tO0hSDZD5pu0F8IIdMDRGssU');
				await doc.useServiceAccountAuth(creds);
				await doc.loadInfo(); 
				const sheet = doc.sheetsByIndex[1];
				const rows = await sheet.getRows({
					offset: 0
				})
				this.rows = rows;
			}
		},
		created() {
			this.accessSpreadSheet();
		}
		
	}
</script>

<style scoped>

</style>