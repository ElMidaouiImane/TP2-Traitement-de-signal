<div align="center">
  <h1 >TP2- Jeux de mots <br/>
Synthèse et analyse spectrale d’une gamme de musique</h1>
</div>

<summary>Table of Contents</summary>
  <ol>             
      <li><a href="#Objectifs">Objectifs</a></li>
      <li><a href="#Jeux-de-mots">Jeux de mots</a></li> 
      <li><a href="#Synthèse-et-analyse-spectrale-d’une-gamme-de-musique">Synthèse-et-analyse-spectrale-d’une-gamme-de-musique</a></li>
       <ul>
         <li><a href="##Synthèse-d’une-gamme-de-musique">Synthèse d’une gamme de musique</a></li> 
         <li><a href="##Spectre-de-la-gamme-de-musique">Spectre de la gamme de musique</a></li>
         <li><a href="##Approximation-du-spectre-d'un-signal-sinusoïdal-à-temps-continu-par-FFT">Approximation du spectre d’un signal sinusoïdal à temps continu par FFT</a></li>
       </ul>      
  </ol>
  
  # Objectifs

> • Comprendre comment manipuler un signal audio avec Matlab, en effectuant
> certaines opérations classiques sur un fichier audio d’une phrase enregistrée via
> un smartphone.
>
> • Comprendre la notion des sons purs à travers la synthèse et l’analyse spectrale
> d’une gamme de musique.
> 
> **Commentaires** : Il est à remarquer que ce TP traite en principe des signaux continus.
> Or, l'utilisation de Matlab suppose l'échantillonnage du signal. Il faudra donc être
> vigilant par rapport aux différences de traitement entre le temps continu et le temps
> discret.
> 
> **Tracé des figures** : toutes les figures devront être tracées avec les axes et les
> légendes des axes appropriés.
> 
> **Travail demandé** : un script Matlab commenté contenant le travail réalisé et des
> commentaires sur ce que vous avez compris et pas compris, ou sur ce qui vous a
> semblé intéressant ou pas, bref tout commentaire pertinent sur le TP.

# Jeux-de-mots

« phrase.wave » est un fichier audio enregistré à l’aide d’un smartphone, en prononçant lentement la phrase :<br/>


 > **« Rien ne sert de courir, il faut partir à point ».** <br/> <br/>
 
1- Sauvegardez ce fichier sur votre répertoire de travail, puis charger-le dans MATLAB à l’aide de la commande « **audioread** ».  

```matlab
[a,f] = audioread("phrase.opus");
```
2- Tracez le signal enregistré en fonction du temps,

```matlab
N = length(a)
ts = 1/f
t = (0:N-1)*ts;
plot(t,a)
title('le signal enregistré')
```
![signal](https://user-images.githubusercontent.com/121026639/210096660-e5853567-5c21-45ea-b5d6-c4a34f4a4327.png)

puis écoutez-le en utilisant la commande « **sound** »
```matlab
sound(a,f)
```
3- Cette commande permet d’écouter la phrase à sa fréquence d’échantillonnage d’enregistrement. Ecoutez la phrase en modifiant la fréquence d’échantillonnage à
double ou deux fois plus petite pour vous faire parler comme « Terminator » ou « Donald Duck ». En effet, modifier la fréquence d’échantillonnage revient à appliquer
un changement d’échelle y(t) = x(at) en fonction de la valeur du facteur d’échelle, cela revient à opérer une compression ou une dilatation du spectre initial d’où la version plus grave ou plus aigüe du signal écouté.
```matlab
sound(a,f*2) % Donald Duck
sound(a,f*0.5) % Terminator
```
4- Tracez le signal en fonction des indices du vecteur x, puis essayez de repérer les indices de début et de fin de la phrase « Rien ne sert de ».

```matlab
riennesertde = a(5055:170900);
plot(riennesertde);
title('Rien ne sert de')
 ```
![partie_new](https://user-images.githubusercontent.com/121026639/210100158-fe35904d-9e5a-4332-9edc-7dad2087d05c.png)

 
 5- Pour segmenter le premier mot, il faut par exemple créer un vecteur « riennesertde » contenant les n premières valeurs du signal enregistré x qui correspondent à ce
morceau. Créez ce vecteur, puis écoutez le mot segmenté.
```matlab
riennesertde = a(5055:170900);
sound(riennesertde,f)
```
6- Segmentez cette fois-ci toute la phrase en créant les variables suivantes : riennesertde, courir, ilfaut, partirapoint.
```matlab
riennesertde = a(5055:170900);
sound(riennesertde,f)

courir = a(172456:250489);
sound(courir,f)

ilfaut = a(250000:334689);
sound(ilfaut,f)

partirapoint = a(334895:485984);
sound(partirapoint,f)
```
7- Notez que le signal initial de parole est un vecteur colonne contenant un certain nombre de valeurs (length(x)). Réarrangez ce vecteur pour écouter la phrase synthétisée « Rien ne sert de partir à point, il faut courir ».
```matlab
vecteur = [riennesertde ; partirapoint ; ilfaut ; courir];
sound(vecteur,f)
```
# Synthèse-et-analyse-spectrale-d’une-gamme-de-musique
 ## Synthèse-d’une-gamme-de-musique
 Les notes de musique produites par un piano peuvent être synthétisées approximativement numériquement. En effet, chaque note peut être considérée comme étant un son pur produit par un signal sinusoïdal. La fréquence de la note « La » est par exemple de 440 Hz.

1- Créez un programme qui permet de jouer une gamme de musique. La fréquence de chaque note est précisée dans le tableau ci-dessous. Chaque note aura une durée
de 1s. La durée de la gamme sera donc de 8s. La fréquence d’échantillonnage fe sera fixée à 8192 Hz.

| Dol  | Ré  | Mi  | Fa  | Sol  | la  | Si  | Do2  |
|---|---|---|---|---|---|---|---|
| 262 Hz  | 294 Hz  | 330 Hz  | 349 Hz  | 392 Hz  | 440 Hz  | 494 Hz  | 523 Hz  |
```matlab
clear all
close all
clc
fe = 8192;
te = 1/fe
t = 0:te:1;
do1 = sin(2*pi*262*t);
re = sin(2*pi*249*t);
mi = sin(2*pi*330*t);
fa = sin(2*pi*349*t);
sol = sin(2*pi*392*t);
la = sin(2*pi*440*t);
si = sin(2*pi*494*t);
do2 = sin(2*pi*523*t);
% 1 note
note1 = input('Donnez la premiére note : \n 1:DO \n 2:RE \n 3:MI \n 4:FA \n 5:SOL \n 6:LA \n 7:SI \n 8:DO(2) \n');
if note1 == 1
    A = do1
elseif note1 == 2
    A = re
elseif note1 == 3
    A = mi
elseif note1 == 4
    A = fa
elseif note1 == 5
    A = sol
elseif note1 == 6
    A = la
elseif note1 == 7
    A = si
elseif note1 == 8
    A = do2
end   
% 2 note
note2 = input('Donnez la deuxième note : \n 1:DO \n 2:RE \n 3:MI \n 4:FA \n 5:SOL \n 6:LA \n 7:SI \n 8:DO(2) \n');
if note2 == 1
    B = do1
elseif note2 == 2
    B = re
elseif note2 == 3
    B = mi
elseif note2 == 4
    B = fa
elseif note2 == 5
    B = sol
elseif note2 == 6
    B = la
elseif note2 == 7
    B = si
elseif note2 == 8
    B = do2
end   
% 3 note
note3 = input('Donnez la troisième note : \n 1:DO \n 2:RE \n 3:MI \n 4:FA \n 5:SOL \n 6:LA \n 7:SI \n 8:DO(2) \n');
if note3 == 1
    C = do1
elseif note3 == 2
    C = re
elseif note3 == 3
    C = mi
elseif note3 == 4
    C = fa
elseif note3 == 5
    C = sol
elseif note3 == 6
    C = la
elseif note3 == 7
    C = si
elseif note3 == 8
    C = do2
end   
% 4 note
note4 = input('Donnez la quatrième note : \n 1:DO \n 2:RE \n 3:MI \n 4:FA \n 5:SOL \n 6:LA \n 7:SI \n 8:DO(2) \n');
if note4 == 1
    D = do1
elseif note4 == 2
    D = re
elseif note4 == 3
    D = mi
elseif note4 == 4
    D = fa
elseif note4 == 5
    D = sol
elseif note4 == 6
    D = la
elseif note4 == 7
    D = si
elseif note4 == 8
    D = do2
end   
% 5 note
note5 = input('Donnez la cinquième note : \n 1:DO \n 2:RE \n 3:MI \n 4:FA \n 5:SOL \n 6:LA \n 7:SI \n 8:DO(2) \n');
if note5 == 1
    E = do1
elseif note5 == 2
    E = re
elseif note5 == 3
    E = mi
elseif note5 == 4
    E = fa
elseif note5 == 5
    E = sol
elseif note5 == 6
    E = la
elseif note5 == 7
    E = si
elseif note5 == 8
    E = do2
end   
% 6 note
note6 = input('Donnez la sixième note : \n 1:DO \n 2:RE \n 3:MI \n 4:FA \n 5:SOL \n 6:LA \n 7:SI \n 8:DO(2) \n');
if note6 == 1
    F = do1
elseif note6 == 2
    F = re
elseif note6 == 3
    F = mi
elseif note6 == 4
    F = fa
elseif note6 == 5
    F = sol
elseif note6 == 6
    F = la
elseif note6 == 7
    F = si
elseif note6 == 8
    F = do2
end   
% 7 note
note7 = input('Donnez la septième note : \n 1:DO \n 2:RE \n 3:MI \n 4:FA \n 5:SOL \n 6:LA \n 7:SI \n 8:DO(2) \n');
if note7 == 1
    G = do1
elseif note7 == 2
    G = re
elseif note7 == 3
    G = mi
elseif note7 == 4
    G = fa
elseif note7 == 5
    G = sol
elseif note7 == 6
    G = la
elseif note7 == 7
    G = si
elseif note7 == 8
    G = do2
end   
% 8 note
note8 = input('Donnez la huitième note : \n 1:DO \n 2:RE \n 3:MI \n 4:FA \n 5:SOL \n 6:LA \n 7:SI \n 8:DO(2) \n');
if note8 == 1
    H = do1
elseif note8 == 2
    H = re
elseif note8 == 3
    H = mi
elseif note8 == 4
    H = fa
elseif note8 == 5
    H = sol
elseif note8 == 6
    H = la
elseif note8 == 7
    H = si
elseif note8 == 8
    H = do2
end 
%la gamme
Gamme = [ A B C D E F G H ];
sound(Gamme,fe)
````

## Spectre-de-la-gamme-de-musique
2- Utilisez l’outil graphique d’analyse de signaux signalAnalyzer pour visualiser le spectre de votre gamme. Observez les 8 fréquences contenues dans la gamme et
vérifiez leur valeur numérique à l’aide des curseurs.

![gamme](https://user-images.githubusercontent.com/121026639/210112002-7287b5e2-160b-4d3f-a48a-d9b47822c0f6.png)

3- Tracez le spectrogramme qui permet de visualiser le contenu fréquentiel du signal au cours du temps (comme le fait une partition de musique) mais la précision sur l’axe
des fréquences n’est pas suffisante pour relever précisément les 8 fréquences.


![spectrogram](https://user-images.githubusercontent.com/121026639/210112553-0f2f1a1d-6855-4550-8116-8ff8e857e30b.png)

## Approximation-du-spectre-d'un-signal-sinusoïdal-à-temps-continu-par-FFT
4- Le spectre d’un signal à temps continu peut être approché par transformée de Fourier discrète (TFD) ou sa version rapide (Fast Fourier Transform (FFT). Afficher le
spectre de fréquence de la gamme musicale crée en échelle linéaire, puis avec une échelle en décibels. 

> S(dB) =20×log₁₀|S(f)|  

##### spectre de fréquence en échelle linéaire

```matlab
N = length(Gamme);
fshift = (-N/2:N/2-1)*(fe/N);
plot(fshift,fftshift(2*abs(fft(Gamme))/N));
title('spectre en échelle linéaire');
```

![linéaire](https://user-images.githubusercontent.com/121026639/210138445-ade79389-0ce8-4499-aed4-3510c1b1dd17.png)

##### spectre de fréquence avec une échelle en décibels

```matlab
plot(fshift,20*log10(fftshift(2*abs(fft(Gamme))/N)));
title('spectre avec une échelle en décibels');
```
![décibels](https://user-images.githubusercontent.com/121026639/210138499-2e6ecc48-f357-44ac-b175-6a60f8676038.png)

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
<h4> Réalisé par : EL MIDAOUI Imane  </h4> <br/> 
 > Encadré par  : [Pr. AMMOUR Alae]


