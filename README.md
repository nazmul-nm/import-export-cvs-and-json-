# import-export-cvs-and-json-
Import as CSV and JSON file from MySQL using CodeIgniter. 
Export from CSV & JSON file and update in to database using Codeigniter
## Import Json
<pre>
  public function import_json($slug){
		$csvMimes = array('application/json','text/json','text/json','text/json');
		if(!empty($_FILES['file']['name']) && in_array($_FILES['file']['type'],$csvMimes)){
			if(is_uploaded_file($_FILES['file']['tmp_name'])){
				$string = file_get_contents($_FILES['file']['tmp_name']);
				$json_a = json_decode($string, true);
				foreach ($json_a as $key => $value) {
					$data[] = array(
							'id' => $value['id'],
							$slug => $value[$slug],
					);
				}
				$this->db->update_batch('language_data',$data, 'id'); 
				$this->session->set_flashdata('success', 'Save Change Successful');
			}else{
				$this->session->set_flashdata('error', lang('error_text'));
			}
		}else{
			$this->session->set_flashdata('error', "Invalid File");
		}
		
		redirect(base_url('admin/auth/whatsapp_message'));
	}

</pre>
## Export JSON
<pre>
public function exportjson($slug){ 
   // file name 
		$filename = $slug.'_'.date('Ymd').'.json'; 
		header("Content-Description: File Transfer"); 
		header("Content-Disposition: attachment; filename=$filename"); 
		header("Content-Type: application/json; charset=UTF-8");
		header('content-type:text/html;charset=utf-8');

   // get data 
		$usersData = $this->admin_m->get_cvs_data();
		$data = array();

		foreach ($usersData as $key => $row) {
			$data[] = array(
				'id' => $row['id'],
				'keyword' => $row['keyword'],
				'english' => $row['english'],
				$slug => !empty($row[$slug])?$row[$slug]:'',
			);
		}


		$fp = fopen('php://output', 'w');
		fwrite($fp, json_encode($data,JSON_PRETTY_PRINT|JSON_UNESCAPED_UNICODE ));
		fclose($fp);


	}
</pre>
