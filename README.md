# praktikum6_web2_php_ci4

Nama : alfaza putra adjie ariefiansyah

NIM  : 312210512

KELAS: TI.22.A5


![Screenshot 2024-06-05 082923](https://github.com/alfaza-putra/praktikum6_web2/assets/129705943/5059f0a1-94b9-447c-81d1-85874482aa48)
![Screenshot 2024-06-05 082945](https://github.com/alfaza-putra/praktikum6_web2/assets/129705943/4bef2f15-f6f9-492c-84fa-afd5e1f43c04)


## AJAX.PHP
```
<?= $this->include('template/header'); ?>

<h1>Data Artikel</h1>

<table class="table-data" id="artikelTable">
    <thead>
        <tr>
            <th>ID</th>
            <th>Judul</th>
            <th>Status</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody></tbody>
</table>

<script src="<?= base_url('assets/js/jquery-3.7.1.min.js') ?>"></script>
<script>
 $(document).ready(function(){
    function showLoadingMessage(){
        $('#artikelTable tbody').html('<tr><td colspan="4">Loading data...</td></tr>');
    }

    function loadData(){
        showLoadingMessage();

        $.ajax({
            url: "<?=base_url('ajax/getData')?>",
            method: "GET",
            dataType: "json",
            success: function(data) {
                var tableBody = "";
                for (var i = 0; i < data.length; i++) {
                    var row = data[i];
                    tableBody += '<tr>';
                    tableBody += '<td>' + row.id + '</td>';
                    tableBody += '<td>' + row.judul + '</td>';
                    tableBody += '<td><span class="status">----</span></td>';
                    tableBody += '<td>';
                    tableBody += '<a href="<?=base_url('artikel/edit/')?>' + row.id + '" class="btn btn-primary">Edit</a>';
                    tableBody += '<a href="#" class="btn btn-danger btn-delete" data-id="' + row.id + '">Delete</a>';
                    tableBody += '</td>';
                    tableBody += '</tr>';
                }
                $('#artikelTable tbody').html(tableBody);
            }
        });
    }
    
    loadData();

    $(document).on('click', '.btn-delete', function(e) {
        e.preventDefault();
        var id = $(this).data('id');

        if(confirm('Apakah anda yakin ingin menghapus artikel ini?'))
        {
            $.ajax({
                url: "<?= base_url('artikel/delete/')?>" + id, method: "DELETE",
                success: function(data){
                    loadData();
                },
                error: function(jqXHR, textStatus, errorThrown) {
                    alert('Error Deleting article: ' + textStatus + errorThrown);
                }
            });
        }
        console.log('Delete button clicked for ID:', id);
    });
});

</script>

<?= $this->include('template/footer'); ?>

```

![Screenshot 2024-06-05 083714](https://github.com/alfaza-putra/praktikum6_web2/assets/129705943/1760c2ce-6805-4c41-be17-b78ef068fd44)

## AJAXCONTROLLER.PHP
```
<?php
namespace App\Controllers;
use CodeIgniter\Controller;
use CodeIgniter\HTTP\Request;
use CodeIgniter\HTTP\Response;
use App\Models\ArtikelModel;
class AjaxController extends Controller
{
public function index()
{
return view('ajax/index');
}
public function getData()
{
$model = new ArtikelModel();
$data = $model->findAll();
// Kirim data dalam format JSON
return $this->response->setJSON($data);
}
public function delete($id)
{
$model = new ArtikelModel();
$data = $model->delete($id);
$data = [
'status' => 'OK'
];
// Kirim data dalam format JSON
return $this->response->setJSON($data);
}
}
```
