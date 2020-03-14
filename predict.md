---
title: prediction
image: pic02.jpg
---

<header>
    <div class="container" id="maincontent" tabindex="-1">
		<div class="row">
			<div class="col-lg-6 ">
				<h2 class="name">Prêt-e à connaître le nombre de like de votre prochaine publication ?</h2>
				<hr class="star-light">
				<h3> Remplissez le formulaire afin de connaître le résultat !</h3>
			</div>
			<div class="col-lg-5 col-lg-offset-1" id="formulaire">
				<form action="/result" method="POST">
					<div class="form-group">
						<label for="publication">Votre publication</label><br>
						<input type="text" id="publication" name="publication">	
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
						<option value="7">Dimanche</option>
						</select>
					</div>
					<div class="form-group">
						<label for="publication_height">hauteur de la publication </label>
						<input type="text" id="publication_height" name="publication_height">
					</div>
					<div class="form-group">
						<label for="publication_is">La publication est </label>
						<select class="custom-select" id="publication_is" name="publication_is">
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
			</div>
		</div>
	</div>
</header>