# Ascon-AEAD128-SystemVerilog

# Conception d'un système numérique : Ascon-AEAD128

##  Présentation du projet
Ce projet consiste à modéliser une version simplifiée de l'algorithme de chiffrement original : l'Ascon-AEAD128. L'algorithme Ascon 128 fait partie de la famille des chiffrements authentifiés légers et a été choisi en 2023 par le NIST (National Institute of Standards and Technology) pour devenir une norme.

L'objectif principal est de représenter le fonctionnement de cet algorithme de manière structurée en utilisant le langage de description matérielle **SystemVerilog**. Le logiciel **ModelSim** a été utilisé pour simuler les différentes composantes ainsi que le circuit final.

##  Principe de fonctionnement
Dans cette version simplifiée d'Ascon, le message traité est composé de 96 bits de données associées (un bloc de 128 bits après padding) et de 376 bits de texte en clair (trois blocs de 128 bits après padding). L'algorithme opère sur un état courant de 320 bits, représenté par 5 registres de 64 bits chacun. 

Le processus est divisé en 4 étapes principales :
* Initialisation de l'état courant S avec la clé de chiffrement K, le vecteur d'initialisation IV et un nombre arbitraire N.
* Traitement des données associées A, consistant à faire une opération XOR avec un bloc de 128 bits.
* Traitement du texte clair P par blocs de 128 bits, avec récupération des blocs de texte chiffré.
* Finalisation des calculs et récupération du Tag T.

La mise à jour de l'état se fait grâce au bloc de permutation qui applique soit 8 itérations ($p^8$), soit 12 itérations ($p^{12}$). Cette permutation est composée de trois transformations : l'addition de constante ($p_C$), la couche de substitution ($p_S$) et la couche de diffusion linéaire ($p_L$).

##  Architecture des fichiers
Les différents modules développés pour ce projet sont les suivants :

* `ascon_fsm.sv` : Ce fichier fait le lien entre les différents modules de l'algorithme et la machine d'état.
* `ascon_pack.sv` : Ce fichier contient la déclaration des 5 registres de 64 bits chacun ainsi que le tableau qui contient les coefficients de ronde.
* `ascon_top.sv` : Ce fichier contient les différents états et conditions de transition de la machine d'état.
* `cipher_register.sv` : Ce fichier contient le registre dans lequel sera contenu le Cipher.
* `constant_addition.sv` : Ce fichier contient le bloc addition de constante de la permutation.
* `cpt_bloc.sv` et `cpt_ronde.sv` : Ces 2 modules permettent respectivement d'implémenter le compteur de bloc et de ronde.
* `diffusion_lin.sv` : Ce fichier contient le bloc couche de diffusion linéaire de la permutation.
* `permut_mem.sv` : Ce fichier contient l'entièreté de la permutation.
* `sbox.sv` : Ce fichier contient les différentes transformations qui seront utilisées dans le module de substitution.
* `state_register.sv` : Ce fichier contient le registre dans lequel les valeurs en sortie de la permutation sont stockées à l'état courant.
* `substitution_layer.sv` : Ce fichier contient le bloc substitution de la permutation.
* `tag_register.sv` : Ce fichier contient le registre dans lequel sera contenu le Tag.
* `xor_down.sv` et `xor_up.sv` : Ces fichiers permettent de faire des opérations de type XOR sur les différents registres de l'état courant.

##  Simulation et Validation
Chaque module a été testé individuellement via des *testbenches* pour valider les comportements face au cahier des charges. La machine d'état finale (FSM), qui pilote les signaux de contrôle de l'algorithme, a également été testée avec succès. Les simulations ont permis de confirmer que les résultats obtenus pour le Cipher et le Tag étaient identiques aux valeurs attendues.
