<template>
	<tr>
		<td>{{row.Nama_Yayasan}}</td>
		<td>{{row.Alamat}}</td>
		<td>{{row.nama_penerima_bantuan}}</td>
		<td>{{row.alamat_rekening}}</td>
    <td>{{row.nama_pic_yayasan}}</td>
    <td>{{row.no_hp}}</td>
    <td>{{row.pic_telkomsel}}</td>
    <td>{{row.jenis_kebutuhan}}</td>
    <td>{{row.nominal}}</td>
    <td>{{row.KATEGORI}}</td>
	</tr>
</template>

<script>
	export default {
		name: "Row",
		props: ["row"],
	}
	
</script>

<style scoped>

</style>