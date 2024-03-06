var user = navigator.userAgent;
var link = document.querySelectorAll("[deepLink]");

if (user.match(/iPad/i))
{
    for (i = 0; i < link.length; i++) {
	  if(link[i].getAttribute("deepLink") !== "") {
	  	link[i].href = link[i].getAttribute("deepLink");
	  }
	}
} 

if (user.match(/Android|webOS|iPhone|iPod|Blackberry/i) ) {
	for (i = 0; i < link.length; i++) {
	  if(link[i].getAttribute("deepLink") !== "") {
	  	link[i].href = link[i].getAttribute("deepLink");
	  }
	}
}