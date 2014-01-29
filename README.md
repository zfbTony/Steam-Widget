SteamWidget
===========

Using this php class I've made utilizes steams API so you can easly create a table of all your Steam games with logos and how much time you've spent playing each game as well as links to the store page of each game you own. You can also display a styled output of your current online status with your screen name and if you're playing a game, the name of that game. Each call requires the steamID64 of the user to query which is sometimes hard to find so i've also included a small script that will convert a SteamID into the SteamID64 for use with making queries. Still working on making it more friendly for multiple users. 

Setup
-----
1. Go to http://steamcommunity.com/dev/apikey and get an api key if you don't already have one.
2. Open SteamWidget.class.php to line 6 and replace the pound symbols with your apikey.
```
define("APIKEY", "################################");
```
3. you can also add a default profile to displace if a SteamID64 errors. 
```
define("DEFAULTPROFILE", "76561198016593929");
```
4. You can added a fallback image for games without a logo, point to the image here.
```
define("NOIMAGE", "css/noimage.jpg");
```


How to use
-----------
<dl>
  <dt>To call username for current online status use these lines</dt>
  <dd>It will echo out current online status (online, offline, away, in-game).
If user is in game it will print out the game title as a link to get the game in the store.</dd>
</dl>
```
  $steamwidget = new SteamWidget();
	echo $steamwidget->current_steam_status();
```

<dl>
  <dt>To call a prestyled table of a user's owned games</dt>
  <dd>This will query steams API to get a list of all games that a user owns with the game's logo, user's playtime data and even a link to the game in the Steam store.</dd>
</dl>
```
  $steamwidget = new SteamWidget();
	echo $steamwidget->query_games_styled($usersSteamID64);
```

<dl>
  <dt>To call a for all of a user's owned games to return as a multidimentional array</dt>
  <dd>array outputs: number, name, appid, playtimeago, gamelogourl (more to come possibly)</dd>
</dl>
```
<table class="table table-striped table-bordered">
	<tr><th>#</th><th>Logo</th><th>Games I Own</th><th>Playtime</th></tr>

	<?php $steamwidget = new SteamWidget();
	$games = $steamwidget->query_games($steamID64ToQuery);
	foreach ($games as $number => $game): ?>
	<tr>
		<td><?php echo $game['number'];?></td>
		<td><img src="<?php echo $game['gamelogourl'];?>" /></td>
		<td><h4><a href="<?php echo 'http://store.steampowered.com/app/' . $game['appid'];?>"><?php echo $game['name'];?></a></h4></td>
		<td><b><?php echo $game['playtimeago'];?></b></td>
	</tr>
	<?php endforeach; ?>
</table>
```

TODO
----
- [X] Add ability to convert SteamID to SteamID64.
- [ ] Create ability to cache recent game list calls to increase page load speed.
- [ ] Make calls to class file dynamic for calling different users from the main page.
