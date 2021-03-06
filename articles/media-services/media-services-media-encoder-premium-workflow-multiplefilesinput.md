<properties 
	pageTitle="Utilisation de plusieurs fichiers d’entrée et propriétés du composant avec Premium Encoder | Microsoft Azure" 
	description="Cette rubrique explique comment utiliser setRuntimeProperties pour plusieurs fichiers d’entrée et transmettre des données personnalisées au processeur multimédia Media Encoder Premium Workflow." 
	services="media-services" 
	documentationCenter="" 
	authors="xpouyat" 
	manager="erikre" 
	editor=""/>

<tags 
	ms.service="media-services" 
	ms.workload="media" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/12/2016"  
	ms.author="xpouyat;anilmur;juliako"/>

#Utilisation de plusieurs fichiers d’entrée et propriétés du composant avec Premium Encoder

##Vue d'ensemble 

Il existe des scénarios dans lesquels vous devrez peut-être personnaliser les propriétés du composant, spécifier le contenu du fichier XML de liste de séquences ou envoyer plusieurs fichiers d’entrée lors de la soumission d’une tâche avec le processeur multimédia **Media Encoder Premium Workflow**. Voici quelques exemples :

- Superposition de texte sur la vidéo et la définition de la valeur de texte (par exemple, la date actuelle) au moment de l’exécution pour chaque vidéo d’entrée.
- Personnalisation du fichier XML de liste de séquences (pour spécifier un ou plusieurs fichiers source, avec ou sans découpage, etc.).
- Superposition d’une image de logo sur l’entrée vidéo pendant l’encodage.

Pour faire savoir à **Media Encoder Premium Workflow** que vous modifiez certaines propriétés dans le flux de travail lors de la création de la tâche ou l’envoi de plusieurs fichiers d’entrée, vous devez utiliser une chaîne de configuration qui contient **setRuntimeProperties** et/ou **transcodeSource**. Cette rubrique explique comment les utiliser.


##Syntaxe de chaîne de configuration

La chaîne de configuration à définir dans la tâche d’encodage utilise un document XML qui ressemble au document suivant :
  
    <?xml version="1.0" encoding="utf-8"?>
    <transcodeRequest>
      <transcodeSource>
      </transcodeSource>
      <setRuntimeProperties>
        <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      </setRuntimeProperties>
    </transcodeRequest>


Le code C# pour lire la configuration XML à partir d’un fichier et la transmettre à la tâche d’un projet est le suivant :

    XDocument configurationXml = XDocument.Load(xmlFileName);
    IJob job = _context.Jobs.CreateWithSingleTask(
                                                  "Media Encoder Premium Workflow",
                                                  configurationXml.ToString(),
                                                  myAsset,
                                                  "Output asset",
                                                  AssetCreationOptions.None);


##Personnalisation des propriétés du composant  

###Propriété avec une valeur simple
Dans certains cas, il peut être utile de personnaliser une propriété du composant avec le fichier de flux de travail qui doit être exécuté par Media Encoder Premium Workflow. Supposons que vous avez conçu un flux de travail qui superpose du texte sur vos vidéos et que ce texte (par exemple, la date actuelle) doive être défini au moment de l’exécution. Pour cela, vous pouvez envoyer le texte en tant que nouvelle valeur pour la propriété de texte du composant Overlay à partir de la tâche d’encodage. Vous pouvez utiliser ce mécanisme pour modifier d’autres propriétés d’un composant dans le flux de travail (par exemple modifier la position ou la couleur de la superposition, modifier le débit binaire de l’encodeur de AVC, etc..). **setRuntimeProperties** est utilisée pour remplacer une propriété dans les composants du flux de travail.

Exemple :

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Optional Overlay/Overlay/filename" value="MyLogo.png"/>
          <property propertyPath="Optional Text Overlay/Text To Image Converter/text" value="Today is Friday the 13th of May, 2016"/>
      </setRuntimeProperties>
    </transcodeRequest>


###Propriété avec une valeur XML

Pour définir une propriété qui attend une valeur XML, encapsulez à l’aide de <![CDATA[ and ]]>.

Exemple :

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

>[AZURE.NOTE]Assurez-vous de ne pas mettre un retour chariot juste après <![CDATA[.


###Valeur propertyPath

Dans les exemples précédents, la valeur propertyPath était « /Media File Input/filename », « /inactiveTimeout » ou « clipListXml ». Il s’agit en général du nom du composant, puis du nom de la propriété. Le chemin d’accès peut avoir différents niveaux, comme « / primarySourceFile » (étant donné que la propriété est à la racine du flux de travail) ou « /Video Processing/Graphic Overlay/Opacity » (car la propriété Overlay est dans un groupe).

Pour vérifier le nom du chemin et de la propriété, utilisez le bouton d’action en regard de chaque propriété. Vous pouvez cliquer sur ce bouton d’action et sélectionner « Modifier». Cette commande affiche le nom réel de la propriété et juste au-dessus, l’espace de noms.

![Action/Modifier](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Propriété](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

##Fichiers d’entrée multiples

Chaque tâche que vous envoyez au **Media Encoder Premium Workflow** nécessite deux éléments :

- Une « ressource de flux de travail » qui contient un fichier de flux de travail pouvant être créé à l’aide du [Concepteur de flux de travail](media-services-workflow-designer.md).
- Un « Élément multimédia » qui contient les fichiers multimédias que vous souhaitez encoder.

Lors de l’envoi de plusieurs fichiers multimédias à l’encodeur **Media Encoder Premium Workflow**, les contraintes suivantes s’appliquent :

- Tous les fichiers multimédias doivent être dans le même « Élément multimédia ». L’utilisation de plusieurs éléments multimédias n’est pas prise en charge.
- Vous devez définir le fichier principal dans cet élément multimédia (dans l’idéal, il s’agit du fichier vidéo principal que l’encodeur doit traiter).
- Il est nécessaire de transmettre les données de configuration qui incluent l’élément **setRuntimeProperties** et/ou **transcodeSource** au processeur.
  - **setRuntimeProperties** est utilisé pour remplacer le nom de fichier ou une autre propriété dans les composants du flux de travail.
  - **transcodeSource** est utilisé pour spécifier le contenu du fichier XML de liste de séquences.

Connexions dans le flux de travail
  - Si vous utilisez un ou plusieurs composants Media File Input et que vous envisagez d’utiliser **setRuntimeProperties** pour spécifier le nom de fichier, ne leur connectez pas la broche du composant de fichier principal. Vérifiez qu’il n’y a aucune connexion entre l’objet de fichier principal et les composants Media File Input.
  - Si vous préférez utiliser le fichier XML de liste de séquences et un composant Media Source, vous pouvez les connecter entre eux.

![Aucune connexion entre la propriété Primary Source File et le composant Media File Input](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Aucune connexion entre le fichier principal et les composants Media File Input si vous utilisez setRuntimeProperties pour définir le nom de fichier*


![Connexion depuis le fichier XML de liste de séquences au composant Clip List Source](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*Vous pouvez connecter le fichier XML de liste de séquences à Media Source et utiliser transcodeSource*


###Personnalisation du fichier XML de liste de séquences
Vous pouvez spécifier le fichier XML de liste de séquences dans le flux de travail, lors de l’exécution, à l’aide de **sourceTranscode** dans la chaîne de configuration XML. Cela nécessite que la broche du fichier XML de liste de séquences soit connectée au composant Media Source dans le flux de travail.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <transcodeSource>
          <clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
          </clipList>
        </transcodeSource>
        <setRuntimeProperties>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Si vous souhaitez spécifier /primarySourceFile pour utiliser cette propriété afin de nommer les fichiers de sortie à l’aide de « Expressions », il est recommandé de transmettre le fichier XML de liste de séquences en tant que propriété, *après* la propriété /primarySourceFile, afin d’éviter que la liste de séquences ne soit remplacée par le paramètre /primarySourceFile.

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Avec découpage précis de la trame supplémentaire :

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
               <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


##Exemple

Prenons un exemple dans lequel vous voulez superposer une image de logo sur la vidéo d’entrée pendant l’encodage. Dans cet exemple, la vidéo d’entrée est nommée « MyInputVideo.mp4 » et le logo est nommé « MyLogo.png ». La procédure suivante doit être effectuée :

- Créez une ressource de flux de travail avec le fichier de flux de travail (exemple ci-dessous).
- Créez un élément multimédia contenant deux fichiers : MyInputVideo.mp4 en tant que fichier principal et MyLogo.png.
- Envoyez une tâche au processeur multimédia Media Encoder Premium Workflow avec les ressources d’entrée ci-dessus et spécifiez la chaîne de configuration suivante.

Configuration :

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="MyLogo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


Dans l’exemple ci-dessus, le nom du fichier vidéo est envoyé au composant Media File Input et à la propriété primarySourceFile, et le nom du fichier de logo est envoyé à un autre composant Media File Input connecté au composant de superposition graphique.

>[AZURE.NOTE]Le nom du fichier vidéo est envoyé à la propriété primarySourceFile. L’objectif est d’utiliser cette propriété dans le flux de travail pour générer le nom de fichier de sortie correct à l’aide d’Expressions, par exemple.


###Création étape par étape du flux de travail pour superposer un logo sur la vidéo     

Voici les étapes pour créer un flux de travail prenant comme entrées deux fichiers : une vidéo et une image. Ce flux de travail superpose l’image sur la vidéo.

Ouvrez le **Concepteur de flux de travail** et sélectionnez **Fichier**-> **Nouvel espace de travail** -> **Plan de transcodage**

Le nouveau flux de travail affiche 3 éléments :

- Fichier source principal
- Fichier XML de liste de séquences
- Fichier/élément multimédia de sortie

![Nouveau workflow d’encodage](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Nouveau workflow d’encodage*


Pour accepter le fichier multimédia d’entrée, commencez par ajouter un composant Media File Input. Pour ajouter un composant au workflow, recherchez-le dans la zone de recherche du référentiel et faites glisser l’entrée souhaitée dans le volet du concepteur.

Ensuite, ajoutez le fichier vidéo à utiliser pour la conception de votre flux de travail. Pour ce faire, cliquez sur l’arrière-plan du volet du Concepteur de flux de travail et recherchez la propriété Primary Source File dans le volet des propriétés à droite. Cliquez sur l’icône de dossier et sélectionnez le fichier vidéo approprié.

![Primary Source File](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Primary Source File*


Ensuite, spécifiez le fichier vidéo dans le composant Media File Input.

![Source Media File Input](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Source Media File Input*


Une fois cette opération effectuée, le composant Media File Input inspectera le fichier et remplira ses broches de sortie afin de refléter le fichier inspecté.

L’étape suivante consiste à ajouter un composant «Video Data Type Updater » pour spécifier l’espace colorimétrique sur Rec.709. Ajouter un composant « Video Format Converter » défini sur Disposition des données/Type de disposition = Planaire configurable. Cela convertit le flux vidéo en un format pouvant être considéré comme une source du composant de superposition.

![video Data Type Updater et Format Converter](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Video Data Type Updater et Format Converter*

![Type de disposition = Planaire configurable](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Type de disposition = Planaire configurable*

Ensuite, ajoutez un composant Video Overlay et connectez la broche de la vidéo (Uncompressed) à la broche de la vidéo (Uncompressed) du composant Media File Input.

Ajoutez un autre composant Media File Input (pour charger le fichier du logo), cliquez sur ce composant et renommez-le « Logo Media File Input », puis sélectionnez une image (un fichier png par exemple) dans la propriété de fichier. Connectez la broche de l’image Uncompressed à la broche de l’image Uncompressed de la superposition.

![Composant Overlay et source du fichier d’image](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Composant Overlay et source du fichier d’image*


Si vous souhaitez modifier la position du logo sur la vidéo (vous voulez par exemple le positionner 10 % en retrait de l’angle supérieur gauche), désactivez l’option « Entrée manuelle ». Vous pouvez désactiver cette option, car vous utilisez un composant Media File Input pour fournir le fichier de logo au composant de superposition.

![Position de superposition](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Position de superposition*


Pour encoder le flux vidéo en H.264, ajoutez les composants AVC Video Encoder et AAC encoder à l’aire du concepteur. Connectez les broches. Configurez le composant AAC encoder et sélectionnez : Conversion du format audio/Présélection : 2.0 (G, D)

![Encodeurs audio et vidéo](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Encodeurs audio et vidéo*


Ajoutez maintenant les composants **ISO Mpeg-4 Multiplexer** et **File Output**, et connectez les broches, comme illustré.

![MP4 Multiplexer et File Output](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*MP4 Multiplexer et File Output*


Vous devez définir le nom du fichier de sortie. Cliquez sur le composant « File Output » et modifiez l’expression du fichier :

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Nom de sortie du fichier](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Nom de sortie du fichier*


Vous pouvez exécuter le flux de travail localement pour vérifier qu’il fonctionne correctement.

Une fois cette opération effectuée, vous pouvez l’exécuter dans Azure Media Services.

Préparez tout d’abord un élément multimédia dans Azure Media Services avec deux fichiers : le fichier vidéo et le logo. Vous pouvez le faire à l’aide des API .NET ou REST. Vous pouvez également le faire avec le portail Azure ou [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).

Ce didacticiel montre comment gérer des éléments multimédias avec AMSE. Il existe deux façons d’ajouter des fichiers à un élément multimédia.

1. Créez un dossier local, copiez-y les deux fichiers et glissez-déplacez le dossier vers l’onglet Élément multimédia.
1. Téléchargez le fichier vidéo en tant qu’élément multimédia, affichez les informations de l’élément, accédez à l’onglet Fichier, puis téléchargez un fichier supplémentaire (logo).

>[AZURE.NOTE]Veillez à définir un fichier principal dans l’élément multimédia (le fichier vidéo principal).

![Fichiers d’élément multimédia dans AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*Fichiers d’élément multimédia dans AMSE*


Sélectionnez l’élément et choisissez de l’encoder avec Premium Encoder. Téléchargez le flux de travail et sélectionnez-le.

Cliquez sur le bouton pour transmettre les données au processeur, puis ajoutez le code XML suivant pour définir les propriétés d’exécution :

![Premium Encoder dans AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*Premium Encoder dans AMSE*


Ensuite, collez les données XML suivantes. Vous devez spécifier le nom du fichier vidéo pour le composant Media File Input et pour la propriété primarySourceFile. Spécifiez le nom de fichier pour le logo.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*


Si vous utilisez le kit de développement logiciel (SDK) .Net pour créer et exécuter la tâche, ces données XML doivent être transmises en tant que chaîne de configuration.

    public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);
  
Une fois la tâche effectuée, le fichier MP4 dans l’élément de sortie affiche la superposition.

![Superposition sur la vidéo](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Superposition sur la vidéo*


Vous pouvez télécharger l’exemple de flux de travail [ici](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).


##Voir aussi 

[Présentation de l’encodage Premium dans Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Utilisation de l’encodage Premium dans Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[Encodage de contenu à la demande avec Azure Media Services](media-services-encode-asset.md#media_encoder_premium_workflow)

[Codecs et formats de Media Encoder Premium Workflow](media-services-premium-workflow-encoder-formats.md)

[Exemples de fichiers de workflow](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)

[Outil Azure Media Services Explorer](http://aka.ms/amse)

##Parcours d’apprentissage de Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Fournir des commentaires

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!---HONumber=AcomDC_0713_2016-->