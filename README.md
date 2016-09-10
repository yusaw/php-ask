please help me with php error below.
I tried to create BBS by PHP, but got two errors below.
line

Warning: array_unshift() expects parameter 1 to be array, boolean given in (line 38)

Fatal error: Call to undefined function save_data() in  (line 39)


<?php
$save_file = dirname(__FILE__)."/bbslog.txt";
$mode = isset ($_GET["mode"]) ? $_GET["mode"] : "show";

switch  ($mode)  {
 case "show" : mode_show() ; break;
 case  "write" : mode_write() ; break;
 default : mode_show() ; break;
}
function mode_show() {
	show_form();
	$log = load_data();
	echo "<ul>";
	foreach ((array)$log as $i) {
		$name = htmlspecialchars($i["name"]);
		$body = htmlspecialchars($i["body"]);
		echo "<li><b style='color:red;'>$name</b>: $body</li>\n";	
	}
	echo "<ul>";
}

function show_form() {
	echo <<< __FORM__
<form>
name: <input type="text" name="name" size="8"/>
sentense: <input type="text" name="body" size="30"/>
<input type="submit" value="write"/>
<input type="hidden" name="mode" value="write"/>
</form><hr/>
__FORM__;
}
function mode_write() {
	if ($_GET["name"] == "" || $_GET["body"] == "") {
		echo "fill in your name";
		exit;
	}
	$log = load_data();
	array_unshift($log, $_GET);
	save_data($log);
	$self = $_SERVER['SCRIPT_NAME'];
	echo "<a href='$self'>are written</a>";	
}
function load_data() {
	global $save_file;
	$log = array();
	if (file_exists($save_file)) {
		$txt = file_get_contents($save_file);
		$log = unserialize($txt);
	}
	return $log;
	function save_data($log) {
		global $save_file;
		$txt = serialize($log);
		file_put_contents($save_file, $txt);
	}
}
