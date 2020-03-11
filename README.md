# Introduction
This code snippet contains a custom validation error messages you need to complete the `insert`, `update` and `delete` functionality from the previous `ACTIVE RECORD` lesson. 

# Instructions

### Step 1
* Update `application/config/config.php` and set the base url of the application `$config['base_url'] = 'http://localhost/my-app';`.

### Step 2
* Update your controller.

<details>
<summary> application/controllers/Example_controller.php </summary>
<br>

```
<?php
defined('BASEPATH') OR exit('No direct script access allowed');

class Example_controller extends CI_Controller 
{
	protected $list_errors = [];
	protected $required = array('name', 'email', 'contact');
	protected $error = false;

	public function __construct()
	{
	   parent::__construct();
	   $this->load->model('example_model', 'model_mentor');
	}

	public function index()
	{

		$data['title'] = 'Active Record - Demo';
		$config["base_url"] = base_url().'example_controller/index';
		$data["mentors_array"] = $this->model_mentor->get_mentor();
		$data["joined_array"] = $this->model_mentor->get_result(1);

		$this->load->view('mentor_view', $data);
	}

	# URL: http://localhost/my-app/index.php/example_controller/create_mentor
	public function create_mentor()
	{
		if($this->input->post('add') !== null ){
			foreach($this->required as $field) {
			  if (empty($this->input->post($field))) {
			  	$this->error = true;
			  	echo ucfirst($field)." is required * <br>";
			  }
		    }

		    if(!$this->error){
		    	echo "Created and Validated";
		    	# https://codeigniter.com/user_guide/database/query_builder.html#inserting-data
		    	# Perform your insert query for `example_model` 
		    	# insert_mentor($this->input->post());
		    }
		}
	    $this->load->view('create_view');
	}

	# URL: http://localhost/my-app/index.php/example_controller/update_mentor/[id]
	public function update_mentor()
	{
		$mentor_id = $this->uri->segment(3) ?? 0;
		$data['mentor_id'] = $mentor_id;
		$data['row_data'] = $this->db->get_where('mentor', array('id' =>$mentor_id))->row();  


		if($this->input->post('update') !== null ){
			foreach($this->required as $field) {
			  if (empty($this->input->post($field))) {
			  	$this->error = true;
			  	echo ucfirst($field)." is required * <br>";
			  }
		    }

		    if(!$this->error){
		    	echo "Updated and Validated";
		    	# https://codeigniter.com/user_guide/database/query_builder.html#updating-data
		    	# Perform your update query for `example_model` 
		    	# update_mentor($this->input->post());
		    }
		}

	    $this->load->view('update_view', $data);
	}

	# URL: http://localhost/my-app/index.php/example_controller/delete_mentor/[id]
	public function delete_mentor() 
	{	
		$mentor_id = $this->uri->segment(3) ?? 0;

		# https://codeigniter.com/user_guide/database/query_builder.html#deleting-data
		# delete_mentor($mentor_id);
	}
}
```
</details>

### Step 3 
* Create these two views.

<details>
<summary> application/views/create_view.php </summary>
<br>

```
<html>
   <head> 
      <title>Add Mentor</title> 
   </head>
   <body>
      <form action = "<?php echo base_url('index.php/example_controller/create_mentor');?>" method = "POST">
         <h5>Insert mentor's detail to database</h5> 
         Mentor's Name: <input type = "text" name = "name" value = "" size = "50" /> <br> 
         Email: <input type = "email" name = "email" value = "" size = "50" />  <br>
         Contact: <input type = "text" name = "contact" value = "" size = "50" /> <br> 
         <div><input type = "submit" name="add" value = "Submit" /></div>  
      </form>  
   </body>
</html>
```
</details>

<details>
<summary> application/views/update_view.php </summary>
<br>

```
<html>
   <head> 
      <title>Update Mentor</title> 
   </head>
   <body>
      <form action = "<?php echo base_url('index.php/example_controller/update_mentor/'.$mentor_id);?>" method = "POST">
         <h5>Update mentor's detail to database</h5> 
         <input type = "hidden" name = "id" value = "<?php echo $row_data->id?>"/>
         Mentor's Name: <input type = "text" name = "name" value = "<?php echo $row_data->name?>" size = "50" /> <br> 
         Email: <input type = "email" name = "email" value = "<?php echo $row_data->email?>" size = "50" />  <br>
         Contact: <input type = "text" name = "contact" value = "<?php echo $row_data->contact?>" size = "50" /> <br> 
         <div><input type = "submit" name="update" value = "Submit" /></div>  
      </form>  
   </body>
</html>
```
</details>

# Finished
Submit a link of the source code to the Google Classroom assignment related to this project.
