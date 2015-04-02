# Example-of-Server-Side-Scripting

<!DOCTYPE html>
<html>
<head>
	<style>
	body{
		width: 100%;
		margin:0;
		padding:0;
	}
        .results{
		
		width: 960px;
		margin-top: 50px;
	}
        .content .title{
		margin-left: auto;
		margin-right: auto;
		width: 285px;
		margin-top:10px;
	}
	
	select,input[type='text']{height: 25px;}
	.content{
		
		width: 700px;
	}
	
	.content .title img {vertical-align:middle}
	.content .title h1 {display: inline;}

	.content .box_search{
		border:2px solid #000;
		padding:10px;
		margin:20px;	
	}
        
        ul {
		list-style: none;
		padding:0px;
		margin: 0px;
	}

	.box_search table{width: 100%;}
	.box_search table tr {height: 30px;}
	.box_search table tr td {border-bottom: 1px solid #dadada;vertical-align: top}
	.box_search table tr td:first-child{font-weight: bold;}
	.box_search table tr:nth-last-child(2) td{border: 0px !important;border-bottom: 0;}
	.box_search .table_buttons td{border: 0px;padding-top: 22px;}
	.box_search .table_buttons td span{float: right;}
	.box_search .table_buttons td input{width: 80px;}
	.box_search .large-input {width: 100%;height: 24px;}
	.box_search .sm-input {width: 60px;}
	.box_search .inline-checkbox li{display: inline;margin-right: 10px;}
	.results .resultTitle{text-align: center;vertical-align: middle;}
	.results .resultTitle h2{margin: 0px;}
	.results .tableresultset{
		width: 100%;
		border: 1px solid #BBBBBB;
		border-collapse: collapse;
	}
	.tableresultset tr{border-bottom: 1px solid #EAEAEA;padding:2px;}
	.tableresultset tr td img{padding: 5px;}
	.tableresultset .resultContent{padding:10px;position: relative;}
	.tableresultset .resultPrice{
		
		bottom: 8px;
	}
        
                               /*                        
            function IsChecked($chkname,$value)
    {
        if(!empty($_POST[$chkname]))
        {
            foreach($_POST[$chkname] as $chkval)
            {
                if($chkval == $value)
                {
                    return $value;
                }
            }
        }
        return NULL;
    }*/

	.error { color:#FF3322;}
	.hide { display: none;}

	</style>
	<script>
	function formvalidation(){
		var isValid = true;
		var form = document.forms["search_pr"];
		var keywords = form['keywords'].value;
		if(keywords == null || keywords =="")
		{
			alert("Please enter value for Key Words");
			return false;
		}

		var min_price_range = form['price_range_from'].value;
		var max_price_range = form['price_range_to'].value;
		if(min_price_range!="" && max_price_range!="")
		{
			if(max_price_range-min_price_range<0)
			{
				document.getElementById("error_price").className = "";
				document.getElementById("error_price").className = "error";
				isValid = false;
			}
			else
			{
				document.getElementById("error_price").className = "hide";
			}
		}

		var shipping_max_handling_time = form['shipping_max_handling_time'].value;
		if(shipping_max_handling_time == '' || shipping_max_handling_time<1)
		{
			document.getElementById("error_max_handling_days").className = "";
			document.getElementById("error_max_handling_days").className = "error";
			isValid = false;
		}
		else
		{
			document.getElementById("error_max_handling_days").className = "hide";
		}

		return isValid;
	}

	function clearForm(){
		var form  = document.forms["search_pr"];
		form.reset();
		form['keywords'].value = "";
		form['price_range_from'].value = "";
		form['price_range_to'].value = "";
		form['shipping_max_handling_time'].value = "";

		document.getElementById("error_max_handling_days").className = "hide";
		document.getElementById("error_price").className = "hide";

		var inputs = document.getElementsByTagName('input');
		for(var i = 0; i<inputs.length; i++){
		  	if(typeof inputs[i].getAttribute === 'function')
		      	inputs[i].checked = false;
		 }

		 var selects = document.getElementsByTagName('select');
		 for(var i = 0; i<selects.length; i++){
		 	 selects[i].options.selectedIndex = 0;
		 }


	}
	</script>
</head>
<body>
  
	<div class="content">
		<div class="title">
			<img src="http://cs-server.usc.edu:45678/hw/hw6/ebay.jpg" width="130" height="101"> <h1>Shopping</h1>
		</div>

<div class="box_search">
<form method="POST" name="search_pr" onsubmit="return formvalidation()">
<table>
<col width="123px">
<tbody>
<tr>
<td>Key Words*:</td>
    <td><input type="text" name="keywords" class="large-input" 
               value='<?php if(isset($_POST['keywords'])) { echo htmlentities ($_POST['keywords']); }?>'>
    </td>
	</tr>
	<tr>
		<td>Price Range:</td>
		<td>from $<input type="number" name="price_range_from" class="sm-input" min="0"
		value="<?php if(isset($_POST['price_range_from'])) { echo htmlentities ($_POST['price_range_from']); }?>"> 
			to $<input type="number" name="price_range_to" class="sm-input" min="0"		value="<?php if(isset($_POST['price_range_to'])) { echo htmlentities ($_POST['price_range_to']); }?>">
		<div class="error hide" id="error_price">Value for from should be less than value for to</div>		</td>			</tr>
		<tr>			<td>Condition:</td>
						<td>
							<ul class="inline-checkbox">
								<li> 
                                    
   
                                    
                                    
                                    
								<input type="checkbox" name="condition[]" value=1000 
                                       
<?php if(isset($_POST['condition'])) { if(in_array(1000,$_POST['condition'])){ echo htmlentities(" checked=checked");}}?>
								> New 
								</li>

								<li> <input type="checkbox" name="condition[]" value=3000 
<?php if(isset($_POST['condition'])) { if(in_array(3000,$_POST['condition'])){ echo htmlentities(" checked=checked");}}?>
								>
								Used 
								</li>

								<li> <input type="checkbox" name="condition[]" value="4000" 
<?php if(isset($_POST['condition'])) { if(in_array(4000,$_POST['condition'])){ echo htmlentities(" checked=checked");}}?>
								>
								Very Good 
								</li>

								<li> <input type="checkbox" name="condition[]" value=5000
<?php if(isset($_POST['condition'])) { if(in_array(5000,$_POST['condition'])){ echo htmlentities(" checked=checked");}}?>
								>
								Good 
								</li>

								<li> <input type="checkbox" name="condition[]" value="6000"
<?php if(isset($_POST['condition'])) { if(in_array(6000,$_POST['condition'])){ echo htmlentities(" checked=checked");}}?>
								>
	Acceptable 		</li>
	</ul>
</td>	</tr>
<tr>
<td>Buying formats:</td>
<td>
<ul class="inline-checkbox">
<li> 
<input type="checkbox" name="buying_formats[]" value="FixedPrice"
	<?php if(isset($_POST['buying_formats'])) { if(in_array('FixedPrice',$_POST['buying_formats']))
	{ echo htmlentities(" checked=checked");}}?>
	>
	Buy It Now 
</li>
<li> 
	<input type="checkbox" name="buying_formats[]" value="Auction"
	<?php if(isset($_POST['buying_formats'])) { if(in_array('Auction',$_POST['buying_formats']))
		{ echo htmlentities(" checked=checked");}}?>
	>
	Auction 
</li>
<li> 
	<input type="checkbox" name="buying_formats[]" value="Classified"
<?php if(isset($_POST['buying_formats'])) { if(in_array('Classified',$_POST['buying_formats']))
	{ echo htmlentities(" checked=checked");}}?>
	>
	Classified Ads 
</li>
		</ul>
	</td>
</tr>
<tr>
	<td>Seller:</td>
	<td>
		<ul class="inline-checkbox">
<li> <input type="checkbox" name="seller" value="true"
<?php if(isset($_POST['seller'])) { if($_POST['seller']=="true")
{ echo htmlentities(" checked=checked");}}?>
>
 Return Accepted 
 </li>
	</ul>
		</td>
</tr>
	<tr>
	<td>Shipping:</td>
	<td>
		<ul>
<li> <input type="checkbox" name="shipping[]" value="FreeShippingOnly"
<?php if(isset($_POST['shipping'])) { if(in_array('FreeShippingOnly',$_POST['shipping']))
	{ echo htmlentities(" checked=checked");}}?>
>
Free Shipping 
</li>
<li> <input type="checkbox" name="shipping[]" value="Expedited"
 <?php if(isset($_POST['shipping'])) { if(in_array('Expedited',$_POST['shipping']))
{ echo htmlentities(" checked=checked");}}?>
 > Expedited shipping available 
 <br> Max Handling Time(days): 
 <input type="number" name="shipping_max_handling_time" class="sm-input" maxlength=4 min="1"
 value="<?php if(isset($_POST['shipping_max_handling_time'])) { echo htmlentities ($_POST['shipping_max_handling_time']); }?>">
	 <div class="error hide" id="error_max_handling_days">Max. Handling Time(days) should be >=1</div>
</li>
			</ul>
		</td>
	</tr>
	<tr>
		<td>Sort by:</td>
		<td>
<select name="sort_by">
<option value="BestMatch"
<?php if(isset($_POST['sort_by'])) { if($_POST['sort_by'] == "BestMatch") { echo htmlentities(" selected=selected");}}
  else{echo htmlentities(" selected=selected");}?>
>
Best Match
</option>
<option value="CurrentPriceHighest"
<?php if(isset($_POST['sort_by'])) { if($_POST['sort_by'] == "CurrentPriceHighest") { echo htmlentities(" selected=selected");}}?>
>
Price: highest first
</option>
<option value="PricePlusShippingHighest"
<?php if(isset($_POST['sort_by'])) { if($_POST['sort_by'] == "PricePlusShippingHighest") { echo htmlentities(" selected=selected");}}?>
>
Price + Shipping: highest first
</option>
<option value="PricePlusShippingLowest"
<?php if(isset($_POST['sort_by'])) { if($_POST['sort_by'] == "PricePlusShippingLowest") { echo htmlentities(" selected=selected");}}?>
>
Price + Shipping: lowest first
</option>
</select>
</td>
</tr>
<tr>
<td>Results Per Page:</td>
<td>
	<select name="results_per_page">
<option value=5
<?php if(isset($_POST['results_per_page'])) { if($_POST['results_per_page'] == 5) { echo htmlentities(" selected=selected");}}
  else{echo htmlentities(" selected=selected");}?>
>
5</option>
<option value=10
<?php if(isset($_POST['results_per_page'])) { if($_POST['results_per_page'] == 10) { echo htmlentities(" selected=selected");}}?>
>
10</option>
<option value=15
<?php if(isset($_POST['results_per_page'])) { if($_POST['results_per_page'] == 15) { echo htmlentities(" selected=selected");}}?>
>
15</option>
<option value=20
<?php if(isset($_POST['results_per_page'])) { if($_POST['results_per_page'] == 20) { echo htmlentities(" selected=selected");}}?>
>
20</option>
</select>
</td>
</tr>
<tr class="table_buttons">
	<td colspan=2>
	<span>
	<input type="button" value="clear" onclick="clearForm()">
	<input type="submit" value="search">
	</span>
	</td>
	</tr>
	</tbody>
	</table>
	</form>
		</div>
	</div>
	<div class="results">
<?php
/*
$xmlFileContents = file_get_contents($ebayUrl);
        $parse= new SimpleXMLElement($xmlFileContents);

        $resptotalresults=$parse->paginationOutput->totalEntries;
        $respcatname=$parse->searchResult->item->primaryCategory->categoryName;
        $respitemurl=$parse->searchResult->item->viewItemURL;
        $respimage=$parse->searchResult->item->galleryURL;
        $resptitle=$parse->searchResult->item->title;
        $respcondition=$parse->searchResult->item->condition->conditionDisplayName;*/



$startend = 'http://svcs.ebay.com/services/search/FindingService/v1';  
$applicationid = 'Universi-4c31-47f8-9343-b859022140e9'; 
//$globalid = 'EBAY-US';

/*$ebayUrl="http://svcs.ebay.com/services/search/FindingService/v1?siteid=0&SECURITY-APPNAME=Universi-4c31-47f8-9343-b859022140e9";
$ebayUrl=$ebayUrl."&OPERATION-NAME=findItemsAdvanced&SERVICEVERSION=1.0.0&RESPONSE-DATA-FORMAT=XML&keywords=";
$ebayUrl=$ebayUrl.$keywordsUrl."&paginationInput.entriesPerPage=".$resultsperpageURL."&sortOrder=".$sortbyURL;*/

$contents;
$call = "$startend?";
$call .= "OPERATION-NAME=findItemsAdvanced";
$call .= "&siteid=0";
$call .= "&SERVICE-VERSION=1.0.0";
$call .= "&SECURITY-APPNAME=$applicationid";
$call .= "&RESPONSE-DATA-FORMAT=XML";

function prep(&$filarray){
	$filter123 = "";
	$i = 0;
 	foreach($filarray as $itemfil) {
 		foreach ($itemfil as $key =>$value) {
			if(is_array($value)) {
					foreach($value as $key1 => $value1) {
						$filter123 .= "&itemFil($i).$key($key1)=$value1";	
					}
			}
			else{
				if($value!=" ")
					$filter123 .= "&itemFil($i).$key=$value";
			}
		}
		$i++;
	}

	return "$filter123";
}



function resultset($keywords,$resp){
	if ($resp->ack == "Success") {
		 $results = '';
		 $count = $resp->paginationOutput->totalEntries;

		 if($count<1){
		 	echo "<div class='resultTitle'><h1>No Results Found</h1></div>";
		 	die();
		 }

		 echo "<table class='tableresultset'><col width='248px'><tbody>";
		 echo "<tr><td colspan=2 class='resultTitle'><h2>".$count." Results for ".$keywords."</h2></td></tr>";

		$results = $resp->searchResult->children();
		foreach ($results as $result) {
			echo "<tr>";
			echo "<td><img src='".$result->galleryURL."' height='287' width='248'></td>";
			echo "<td class='resultContent'>";
			echo "<a href='".$result->viewItemURL."' >".$result->title."</a><br>";
			echo "<b>Condition:</b> ".$result->condition->conditionDisplayName;
			
			if($result->topRatedListing == "true")
				echo "<img src='http://cs-server.usc.edu:45678/hw/hw6/itemTopRated.jpg' width='50' style='vertical-align:top;'><br>";
			else
				echo "<br><br>";

			switch($result->listingInfo->listingType){
				case "FixedPrice":
				case "StoreInventory":
					echo "<b>Buy It Now</b>";
					break;
				case "Auction":
					echo "<b>Auction</b>";
					break;
				case "Classified"
					echo "<b>Classified</b>";
					break;
			}

			if($result->returnsAccepted == "true")
				echo "<br><br> Seller accepts returns";
			else
				echo "<br><br> Seller doesn't accept returns";


			if($result->shippingInfo->shippingServiceCost == 0.0)
				echo "<br> FREE Shipping --";
			else
				echo "<br> Shipping Not FREE --";

			if($result->shippingInfo->expeditedShipping == "true")
				echo " Expedited Shipping available --";
			else
				echo " Expedited Shipping not available --";

			echo " Handled for shipping in ".$result->shippingInfo->handlingTime." day(s)<br><br><br>";

			echo "<div class='resultPrice'><b>Price:$".$result->sellingStatus->convertedCurrentPrice."</b>";
			if($result->shippingInfo->shippingServiceCost > 0)
				echo " (+ $".$result->shippingInfo->shippingServiceCost." for shipping)";
			echo " From ".$result->location."</div>";

			echo "</td>";
			echo "</tr>";
		}

		echo "</tbody></table><br><br>";


	}
	else
	{
		echo "<div class='resultTitle'><h1>Erorr In Connection, Please Try Again..</h1></div>";
	}
}

function originalform($FORM){

		$keywords = urldecode($FORM['keywords']);
		$GLOBALS['call'] .= "&keywords=$keywords";

		$sortBy = $FORM['sort_by'];
		$GLOBALS['call'] .= "&sortOrder=$sortBy";

		$resultsPerPage = $FORM['results_per_page'];
		$GLOBALS['call'] .= "&paginationInput.entriesPerPage=$resultsPerPage";

		$filarray = array();
		if($FORM['price_range_from']!='')
		{
			array_push($filarray, array(
					    'name' => 'MinPrice',
					    'value' => $FORM['price_range_from'],
					    'paramName' => 'Currency',
					    'paramValue' => 'USD'
			));
		}

		if($FORM['price_range_to']!='')
		{
			array_push($filarray, array(
					    'name' => 'MaxPrice',
					    'value' => $FORM['price_range_to'],
					    'paramName' => 'Currency',
					    'paramValue' => 'USD'
			));
		}

		if(isset($FORM['condition']))
		{
			array_push($filarray, array(
					    'name' => 'Condition',
					    'value' => $FORM['condition'],
			));
		}		
		
		if(isset($FORM['buying_formats']))
		{
			array_push($filarray, array(
					    'name' => 'ListingType',
					    'value' => $FORM['buying_formats'],
			));
		}	

		if(isset($FORM['seller']))
		{
			array_push($filarray, array(
					    'name' => 'ReturnsAcceptedOnly',
					    'value' => 'true',
			));
		}	

		if(isset($FORM['shipping']))
		{	
			if(in_array("FreeShippingOnly",$FORM['shipping']))
			{
				array_push($filarray, array(
					    'name' => 'FreeShippingOnly',
					    'value' => 'true',
				));
			}

			if(in_array("Expedited",$FORM['shipping']))
			{
				array_push($filarray, array(
					    'name' => 'ExpeditedShippingType',
					    'value' => 'Expedited',
				));
			}
		}	

		if($FORM['shipping_max_handling_time']!='')
		{
				array_push($filarray, array(
					    'name' => 'MaxHandlingTime',
					    'value' => $FORM['shipping_max_handling_time'],
				));
		}

if(sizeof($filarray)>0)
{	
	$filter123 = prep($filarray);
	$GLOBALS['call'] .= $filter123;
}
    $resp = simplexml_load_file($GLOBALS['call']);
    resultset($keywords,$resp);
}
?>        
<?php
if($_POST)
originalform($_POST);
?>
	</div>
</body>
</html>
