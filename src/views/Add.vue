<template>
	<div class="container-fluid">
		<div class="row">
			<main role="main" class="col-md-12 ml-sm-auto col-lg-12 pt-3 px-4">
				<div class="container">
<!--					<div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pb-2 mt-3 ">
						<h4 class="mb-3">New row</h4>
						<div class="btn-toolbar mb-2 mb-md-0">
							<router-link to="/"  class="btn btn-sm mr-1 btn-outline-secondary">
								Back
							</router-link>
							<a 	href="https://docs.google.com/spreadsheets/d/1oWHAFng2KoKBqYokzi4tO0hSDZD5pu0F8IIdMDRGssU/edit?usp=sharing"
								class="btn btn-sm btn-outline-secondary"
								target="_blank">
							View Google Sheet
							</a>
						</div>
					</div>-->
					<div v-if="showMsg" class="alert alert-success alert-dismissible fade show" role="alert">
						{{message}}
						<button type="button" class="close" data-dismiss="alert" aria-label="Close">
							<span aria-hidden="true">&times;</span>
						</button>
					</div>
					<div v-else class="col-md-12 order-md-1">
						<form @submit.prevent="addRow">
							<div class="row">
                <div class="col-md-6 mb-3">
                  <label for="kriteria">Kategori</label>
                  <select v-model="kriteria" class="custom-select d-block w-100" id="kriteria" required>
                    <option v-for="(nama, index) in namaKuota" :key="index" :value="index">{{nama}}</option>
                  </select>
                </div>
								<div class="col-md-6 mb-3">
									<label for="name">Nama Yayasan</label>
									<input type="text" v-model="Nama_Yayasan" class="form-control" id="name" required>
								</div>
								<div class="col-md-6 mb-3">
									<label for="address">Alamat</label>
									<input type="text" v-model="Alamat" class="form-control" id="address" required>
								</div>
								<div class="col-md-4 mb-3">
									<label for="nama_penerima">Nama Penerima Bantuan</label>
									<input type="text" v-model="nama_penerima_bantuan" class="form-control" id="nama_penerima" required>
								</div>
								<div class="col-md-2 mb-3">
									<label for="rekening">Alamat Rekening</label>
									<input type="text" v-model="alamat_rekening" class="form-control" id="rekening" placeholder="ex: 16000" required>
								</div>
                <div class="col-md-6 mb-3">
                  <label for="nama_pic_yayasan">Nama PIC Yayasan</label>
                  <input type="text" v-model="nama_pic_yayasan" class="form-control" id="nama_pic_yayasan" required>
                </div>
                <div class="col-md-6 mb-3">
                  <label for="no_hp">No Hp</label>
                  <input type="number" v-model="no_hp" class="form-control" id="no_hp" required>
                </div>
                <div class="col-md-6 mb-3">
                  <label for="pic_telkomsel">PIC Telkomsel</label>
                  <select v-model="pic_telkomsel" class="custom-select d-block w-100" id="pic_telkomsel" required>
                    <option disabled>Choose...</option>
                    <option value="Corcom">Corcom</option>
                    <option value="PD">PD</option>
                  </select>
                </div>
                <div class="col-md-6 mb-3">
                  <label for="jenis_kebutuhan">Jenis Kebutuhan</label>
                  <select v-model="jenis_kebutuhan" class="custom-select d-block w-100" id="jenis_kebutuhan" required>
                    <option disabled>Choose...</option>
                    <option value="Sembako">Sembako</option>
                    <option value="Dana Tunai">Dana Tunai</option>
                    <option value="Dll">Dll</option>
                  </select>
                </div>
                <div class="col-md-6 mb-3">
                  <label for="nominal">Nominal (Rp)</label>
                  <input type="number" v-model="nominal" class="form-control" id="nominal" required>
                </div>
								<div class="col-md-6">
									<label for="">.</label>
									<button class="btn btn-primary btn-block" type="submit">SUBMIT</button>
								</div>
								
							</div>		
						</form>
					</div>

					
				</div>
				
			</main>
		</div>
	</div>
</template>

<script>
	const { GoogleSpreadsheet } = require('google-spreadsheet');
	const creds = require('@/client_secret.json');
	export default {
		name: "AddRow",
		data() {
			return {
        namaKuota: [],
        kuota: [],
        Nama_Yayasan: '',
        Alamat: '',
        nama_penerima_bantuan: '',
        alamat_rekening: '',
        nama_pic_yayasan: '',
        no_hp: '',
        pic_telkomsel: '',
        jenis_kebutuhan: '',
        nominal: '',
        kriteria: '',
				showMsg : false,
				message: '',
			}
		},
		methods: {
      async getKuota() {
        const doc = new GoogleSpreadsheet('1oWHAFng2KoKBqYokzi4tO0hSDZD5pu0F8IIdMDRGssU');
        await doc.useServiceAccountAuth(creds);
        await doc.loadInfo();
        const sheet = doc.sheetsByIndex[2];
        const rows = await sheet.getRows({
          offset: 0
        })

        const kuotakat = [];
        rows.map((x) => {
          let i = true
          if(x.Real >= x.TOTAL){
            i = false;
          }else{
            this.namaKuota.push(x.NAMA)
            kuotakat.push(i)
          }
        })
        this.kuota = kuotakat;
      console.log(kuotakat)
        console.log(this.namaKuota)
      },
			async addRow() {
				const newRow = {
          Nama_Yayasan: this.Nama_Yayasan,
          Alamat: this.Alamat,
          nama_penerima_bantuan: this.nama_penerima_bantuan,
          alamat_rekening: this.alamat_rekening,
          nama_pic_yayasan: this.nama_pic_yayasan,
          no_hp: this.no_hp,
          pic_telkomsel: this.pic_telkomsel,
          jenis_kebutuhan: this.jenis_kebutuhan,
          nominal: this.nominal,
          kriteria_penerima: this.kriteria,
				}

				const doc = new GoogleSpreadsheet('1oWHAFng2KoKBqYokzi4tO0hSDZD5pu0F8IIdMDRGssU');
				await doc.useServiceAccountAuth(creds);
				await doc.loadInfo(); 
				const sheet = doc.sheetsByIndex[1];
				await sheet.addRow(newRow);
        this.getKuota();
				this.name = '';
				this.phone = '';
				this.address = '';
				this.city = '';
				this.zip = '';
				this.activity = '';

				this.message = "New row added !";
				this.showMsg = true;

				
			}
		},
    created() {
      this.getKuota();
    }
	}
</script>

<style scoped>

</style>
