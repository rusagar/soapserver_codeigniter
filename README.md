soapserver_codeigniter
======================

Creating a SOAP server in CodeIgniter


NuSOAP server will be a controller which does not need to have its own view file, since it will only be accessed to perform certain tasks (even interact with the models if it would be necessary) and return their results via web to its remote client, so at no time it will be necessary to show the data on the screen directly from the server.

We now have to create the file “nuSoapServer.php” into the folder “application / controller” in which we insert the following code:


## Quick start:
```
<?php
class NuSoapServer extends CI_Controller {
	function __construct() {
		parent::__construct();
		$this->load->library("nuSoap_lib");
		$this->nusoap_server = new soap_server();
		$this->nusoap_server->configureWSDL("cartWSDL", "urn:cartWSDL");
		$this->nusoap_server->wsdl->addComplexType(
			"Member",
			"complexType",
			"array",
			"",
			"SOAP-ENC:Array",
			array(
				"id"=>array("name"=>"id", "type"=>"xsd:int"),
				"first_name"=>array("name"=>"first_name", "type"=>"xsd:string"),
				"surname"=>array("name"=>"surname", "type"=>"xsd:string")
				)
		);
		$this->nusoap_server->register(
			"getMember",
			array(
			"id" => "xsd:int",
		),
		array("return"=>"tns:Member"),
			"urn:cartWSDL",
			"urn:cartWSDL#getMember",
			"rpc",
			"encoded",
			"Returns the information of a certain member"
		);
	}
	function index() {
		if($this->uri->segment(3) == "wsdl") {
			$_SERVER['QUERY_STRING'] = "wsdl";
		} else {
			$_SERVER['QUERY_STRING'] = "";
		}
		$this->nusoap_server->service(file_get_contents("php://input"));
	}
	
	function get_member() {
		function getMember($id) {
			echo "True";
			/* //in this example i just return "True" ..if you want to access next model and fetch the DB value, it is written here
			$CI =& get_instance();
			$CI->load->model("Customer");
			$row = $CI->Customer->getMemberInfo($id);
			return $row;
			*/
		}
	$this->nusoap_server->service(file_get_contents("php://input"));
	}
}
?>



```
