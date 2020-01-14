---
title: prediction
---

<!--form action="https://formspree.io/{{ site.email }}" method="POST">
	<div class="fields">
		<div class="field half first">
			<label for="name">Name</label>
			<input type="text" name="name" id="name" />
		</div>
		<div class="field half">
			<label for="email">Email</label>
			<input type="text" name="_replyto" id="email" />
		</div>
		<div class="field">
			<label for="message">Message</label>
			<textarea name="message" id="message" rows="4"></textarea>
		</div>
	</div>
	<ul class="actions">
		<li><input type="submit" value="Send Message" class="primary" /></li>
		<li><input type="reset" value="Reset" /></li>
	</ul>
</form-->

<!--FORMULAIRE PROJET IRONHACK-->

<form action="/docs/result" method="POST">
	<div class="form-group">
		<label for="publication">Votre publication</label>
		<textarea id="publication" name="publication"rows="5" cols="33"></textarea>
	</div>
	<div class="form-group">
		<label for="code_jour_semaine">Jour de publication </label>
		<select class="custom-select" id="code_jour_semaine" name="code_jour_semaine">
			<option value="1">Lundi</option>
			<option value="2">Mardi</option>
			<option value="3">Mercredi</option>
			<option value="4">Jeudi</option>
			<option value="5">Vendredi</option>
			<option value="6">Samedi</option>
			<option value="7">Dimanche test</option>
		</select>
	</div>
	<div class="form-group">
		<label for="publication_height">hauteur de la publication </label>
		<input type="text" id="publication_height" name="publication_height">
	</div>
	<div class="form-group">
		<label for="publication_is">La publication est </label>
		<select class="custom-select id="publication_is" name="publication_is">
			<option value="0">une image</option>
			<option value="1">une vidéo</option>
			<option value="2">un diaporama</option>
		</select>
	</div>
	<div class="form-group">
		<label for="tranche_horaire">Heure de publication </label>
		<input type="text"  id="tranche_horaire" name="tranche_horaire" >
	</div>
	<div class="form-group">
		<label for="edi_lib">Editeur ou Libraire</label>
		<select class="custom-select" id="edi_lib" name="edi_lib">
			<option value="0">Editeur</option>
			<option value="1">Librairie</option>
		</select>
	</div>
	<div class="form-group">
		<label for="page_nb_followers_log">Nombre de followers</label>
		<input type="text" id="page_nb_followers_log" name="page_nb_followers_log">
	</div>
	<input class="btn btn-primary" type="submit" value="Submit">
	<br><br>
</form>
