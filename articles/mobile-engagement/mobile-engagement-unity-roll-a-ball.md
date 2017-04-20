<properties
    pageTitle="Einheit zurücksetzen Ball-Lernprogramm"
    description="Schritte zum Erstellen der klassischen eins Rollen ein Spiel ist Voraussetzung für alle Mobile Engagement Einheit Lernprogramme"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a id="unity-roll-a-ball"></a>Erstellen Sie ein Spiel Einheit zurücksetzen

Dieses Lernprogramm führt die wichtigsten Schritte für eine leicht abgewandelte [Einheit Zurücksetzen einer Kugel Tutorial](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial). Dieses Beispiel besteht aus einem kugelförmigen "" Playerobjekt die app Benutzer gesteuert wird und das Ziel des Spiels ist "Sammler Objekte sammeln" durch das Player-Objekt mit dieser entladbaren Objekte kollidieren. Dies setzt Grundkenntnisse in Einheit-Umgebung voraus. Wenn Sie alle Probleme sollten Sie vollständige Tutorial verweisen. 

### <a name="setting-up-the-game"></a>Das Spiel
Schritte werden aus dem [Unity-Lernprogramm](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)

1. Öffnen Sie **Einheit-Editor** und auf **neu**. 
    
    ![][51] 
    
2. Geben Sie einen **Projektnamen** & **Speicherort**wählen **3D** und klicken Sie auf **Projekt erstellen**.
    
    ![][52]

3. Die soeben erstellte neuen Projekt mit dem Namen **Minispiel** in ein neues Standard-Szene speichern ** \_Szenen** Ordner **Ordner** :
    
    ![][53]

4. Erstellen Sie ein **3D-Objekt -> Ebene** als Spielfeld und benennen Sie diese Ebenenobjekt als **Basis**

    ![][1]

5. Setzen Sie die Transformation Komponente für dieses Objekt **Boden** so am Ursprung. 

    ![][3]

6. Deaktivieren Sie **Raster anzeigen** **Zukunftsvisionen** aus **Boden** -Objekts.

    ![][4]

7. **Skalierung** Komponente der **Boden** Objekt aktualisieren [X = 2, Y = 1, Z = 2]. 

    ![][5]

8. Dem Projekt fügen Sie eine neue **3D-Objekt-Bereich > hinzu** und benennen Sie diese Kugel **Spieler**. 

    ![][6]

9. Wählen Sie das **Player** -Objekt und auf das Ebenenobjekt ähnlich **Transformation zurücksetzen** . 

10. Update **Transformation-Position > -> Y-Koordinate** Komponenten für den Player als 0,5.  

    ![][7]

11. Erstellen Sie einen neuen Ordner namens **Materialien** im Projekt, das Material zu den Spieler Farbe erstellen. 

12. Erstellen Sie ein neues **Material** namens **Hintergrund** in diesem Ordner. 

    ![][8]

13. Aktualisieren Sie die Farbe des Materials durch die **Albedo** -Eigenschaft aktualisieren. Wählen Sie die RGB-Werte [0,32,64]. 

    ![][9]

14. Ziehen Sie dieses Material an Szene Farbe auf den **Boden** -Objekt anwenden. 

    ![][10]

17. Schließlich aktualisieren die **Transformation-Rotation > -> Y** 60 gerichtetes Licht Objekt aus Gründen der Übersichtlichkeit. 

    ![][12]

### <a name="moving-the-player"></a>Verschieben des Players
Schritte werden aus dem [Unity-Lernprogramm](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)

1. Fügen Sie eine **RigidBody** -Komponente das **Player** -Objekt. 

    ![][13]

2. Erstellen Sie einen neuen Ordner **Skripts** im Projekt. 

3. Klicken Sie auf **Komponente hinzufügen -> New Script -> C#-Skript**. Namen Sie **PlayerController**, und klicken Sie auf **Erstellen und hinzufügen**. Erstellen und fügen ein Skript auf das Player-Objekt.  

    ![][14]

5. Verschieben Sie dieses Skript im Ordner **Skripts** im Projekt. 

6. Öffnen Sie das Skript für die Bearbeitung in Ihrem bevorzugten Skripteditor, aktualisieren Sie Skriptcode durch den folgenden Code und speichern. 

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour 
        {
            public float speed;
            private Rigidbody rb;
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
                rb.AddForce (movement * speed);
            }
        }
    
8. Beachten Sie, dass das oben angegebene Skript **Speed** -Eigenschaft verwendet. Aktualisieren Sie im Editor Einheit Speed-Eigenschaft auf 10.  

    ![][15]

9. Drücken Sie **Spielen** im Unity-Editor. Jetzt sollte man die Kugel über die Tastatur steuern und sollte drehen und verschieben. 

### <a name="moving-the-camera"></a>Verschieben der Kamera
Schritte aus der [Einheit Lernprogramm](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) und binden die **Main Kamera** das **Player** -Objekt. 

1. Aktualisieren der **Transform.Position** um x = 0, Y = 10,5 Z =-10.  
2. Aktualisieren der **Transform.Rotation** um x = 45, Y = 0, Z = 0.  

    ![][16]

2. Fügen Sie ein neues Skript namens **CameraController** , **MainCamera** und im Skriptordner verschieben. 

    ![][17]

3. Öffnen Sie das Skript bearbeiten und fügen den folgenden Code hinzu:

        using UnityEngine;
        using System.Collections;
        
        public class CameraController : MonoBehaviour {
        
            public GameObject player;
        
            private Vector3 offset;
        
            void Start ()
            {
                offset = transform.position - player.transform.position;
            }
            
            void LateUpdate ()
            {
                transform.position = player.transform.position + offset;
            }
        }
    
5. Ziehen Sie in Einheit Umgebung - Player-Variable in den Player Steckplatz für Main Kameraobjekt mit einander. 

    ![][18]

6. Jetzt Spielen im Editor Einheit erreicht und Player Ball Objekt drehen sehen die Kamera nach der Bewegung Sie.  

### <a name="setting-up-the-play-area"></a>Der Wiedergabebereich einrichten
Schritte werden aus dem [Unity-Lernprogramm](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141). Wir werden die Mauern Boden, so dass Player Ball-Objekt aus der Wiedergabe in der Bewegung nicht erstellen. 

1. Klicken Sie auf **Erstellen -> leer erstellen-Spiels >** Namen **Wände**

    ![][19]

2. Erstellen Sie unter diesem Objekt Wände - einer neuen **3D-Objekts -> Cube** , und nennen Sie sie "westwand". 

    ![][20]

3. Aktualisieren Sie die **Transform -> Position** und **Transform -> Skalierung** für dieses Objekt Westwand. 

    ![][21]

4. Doppelte westwand erstellen eine **ostwand** mit aktualisierten Transformation positionieren und skalieren. 

    ![][22]

5. Doppelte Osten Wand erstellen **North Wall** mit aktualisierten Transformation Position und Skalierung. 

    ![][23]

6. Doppelte North Wall und **South Wand** mit aktualisierten Transformation Position und Skalierung. 

    ![][24]

### <a name="creating-collectible-objects"></a>Entladbaren Objekte erstellen
Schritte werden aus dem [Unity-Lernprogramm](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141). Wir erstellen attraktiv gestaltete Objekte die entladbaren Objektgruppe die Spieler Ball Objekt bilden "sammeln" mit ihnen. 

1. Erstellen Sie eines neuen **Cubes 3D-Objekt** und nennen Sie Abholung. 

2. Anpassen der **Transform-Rotation >** & des Pickup-Objekts**Transformieren -> skalieren** . 

    ![][25]

3. Erstellen Sie und fügen Sie ein **Neues C#-Skript** namens **Rotator** Pickup-Objekt. Stellen Sie das Skript unter dem Ordner Skripts ab. 

    ![][26]

4. Öffnen Sie dieses Skript zum Bearbeiten und aktualisieren Sie, um die folgenden: 

        using UnityEngine;
        using System.Collections;
        
        public class Rotator : MonoBehaviour {
        
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }

5. Jetzt werden der Wiedergabemodus in den Unity-Editor und die Pickup-Objekt drehen auf der Achse.

6. Erstellen Sie einen neuen Ordner namens **Prefabs** 

    ![][27]

7. Ziehen Sie das Objekt **Abholung** und in den Ordner Prefabs.

    ![][28]

8. Erstellen Sie ein neues **leeres Spiels** **Pickups**. Ursprung die Position zurücksetzen und Pickup Gegenstand dieses Spiel Objekt ziehen.  

    ![][29]

9. **Abholung** Objekt duplizieren und aktualisieren die Werte **X und Z Transform.Position** entsprechend auf dem **Boden** Objekt um das **Player** -Objekt verteilt sind. 

    ![][30]

10. Erstellen Sie ein **Neues Material** namens **Abholung** und aktualisieren Sie es rote Farbe ähnlich wie wir zum Aktualisieren der Gegenstand auf dem Boden **Albedo Eigenschaft** aktualisieren. 

    ![][31]

11. Das Material auf alle 4 pickup Objekte anwenden.

    ![][32]

### <a name="collecting-the-pickup-objects"></a>Sammeln der Pickup-Objekte
Schritte werden aus dem [Unity-Lernprogramm](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141). Es wird den Player aktualisiert, sodass "pickup Objekte sammeln" mit ihnen kann. 

1. **PlayerController** Skript angefügt, das Player-Objekt zur Bearbeitung öffnen und wie folgt aktualisieren:  

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour {
        
            public float speed;
        
            private Rigidbody rb;
        
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
        
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
        
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
        
                rb.AddForce (movement * speed);
            }
        
            void OnTriggerEnter(Collider other) 
            {
                if (other.gameObject.CompareTag ("Pick Up"))
                {
                    other.gameObject.SetActive (false);
                }
            }
        }

2. Erstellen Sie ein neues **Tag** **Pick Up** (es muss im Skript Neuigkeiten)  

    ![][33]
    
    ![][34]

3. Dieses **Tag** vorgefertigten Pickup-Objekt anwenden. 

    ![][35]

4. Aktivieren Sie Kontrollkästchen für das Fertighaus-Objekt **IsTrigger** .

    ![][36]

5. Abholung vorgefertigten Objekt Starrkörper hinzufügen. Optimierung der Leistung wird aktualisiert den statischen Collider verwendet, eine dynamische Collider. 

    ![][37]
  
6. Schließlich überprüfen Sie die **IsKinematic** -Eigenschaft vorgefertigten Objekts. 

    ![][38]

7. Hit **Spielen** Unity-Editor und können das **eine Kugel** Spiel durch das Player-Objekt mit den Tasten der Tastatur für die Eingabe Richtung verschieben. 

### <a name="updating-the-game-for-mobile-play"></a>Aktualisieren das Spiel für mobile spielen
In den obigen Abschnitten geschlossenen Lernprogramm Einheit. Jetzt werden wir das Spiel zu mobilen Gerät angezeigten ändern. Beachten Sie, dass Tastatureingaben für das Spiel für Tests verwendet. Jetzt wird es geändert, damit wir den Player steuern können, z. B. mit der Bewegung des Telefons Beschleunigungsmesser verwenden als Eingabe. 

**PlayerController** Skript zur Bearbeitung öffnen Sie und aktualisieren Sie die **FixedUpdate** -Methode, um die Bewegung von der Beschleunigungsmesser verwenden das Player-Objekt verschieben. 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

Dieses Lernprogramm schließt die Erstellung ein grundlegendes Spiel mit und auf einem Gerät Ihrer Wahl spielen können bereitstellen. 

<!-- Images -->
[1]: ./media/mobile-engagement-unity-roll-a-ball/1.png  
[2]: ./media/mobile-engagement-unity-roll-a-ball/2.png
[3]: ./media/mobile-engagement-unity-roll-a-ball/3.png
[4]: ./media/mobile-engagement-unity-roll-a-ball/4.png
[5]: ./media/mobile-engagement-unity-roll-a-ball/5.png
[6]: ./media/mobile-engagement-unity-roll-a-ball/6.png
[7]: ./media/mobile-engagement-unity-roll-a-ball/7.png
[8]: ./media/mobile-engagement-unity-roll-a-ball/8.png
[9]: ./media/mobile-engagement-unity-roll-a-ball/9.png  
[10]: ./media/mobile-engagement-unity-roll-a-ball/10.png    
[11]: ./media/mobile-engagement-unity-roll-a-ball/11.png    
[12]: ./media/mobile-engagement-unity-roll-a-ball/12.png
[13]: ./media/mobile-engagement-unity-roll-a-ball/13.png
[14]: ./media/mobile-engagement-unity-roll-a-ball/14.png
[15]: ./media/mobile-engagement-unity-roll-a-ball/15.png
[16]: ./media/mobile-engagement-unity-roll-a-ball/16.png
[17]: ./media/mobile-engagement-unity-roll-a-ball/17.png
[18]: ./media/mobile-engagement-unity-roll-a-ball/18.png
[19]: ./media/mobile-engagement-unity-roll-a-ball/19.png    
[20]: ./media/mobile-engagement-unity-roll-a-ball/20.png    
[21]: ./media/mobile-engagement-unity-roll-a-ball/21.png    
[22]: ./media/mobile-engagement-unity-roll-a-ball/22.png    
[23]: ./media/mobile-engagement-unity-roll-a-ball/23.png    
[24]: ./media/mobile-engagement-unity-roll-a-ball/24.png    
[25]: ./media/mobile-engagement-unity-roll-a-ball/25.png    
[26]: ./media/mobile-engagement-unity-roll-a-ball/26.png    
[27]: ./media/mobile-engagement-unity-roll-a-ball/27.png    
[28]: ./media/mobile-engagement-unity-roll-a-ball/28.png    
[29]: ./media/mobile-engagement-unity-roll-a-ball/29.png    
[30]: ./media/mobile-engagement-unity-roll-a-ball/30.png    
[31]: ./media/mobile-engagement-unity-roll-a-ball/31.png    
[32]: ./media/mobile-engagement-unity-roll-a-ball/32.png    
[33]: ./media/mobile-engagement-unity-roll-a-ball/33.png    
[34]: ./media/mobile-engagement-unity-roll-a-ball/34.png    
[35]: ./media/mobile-engagement-unity-roll-a-ball/35.png    
[36]: ./media/mobile-engagement-unity-roll-a-ball/36.png    
[37]: ./media/mobile-engagement-unity-roll-a-ball/37.png    
[38]: ./media/mobile-engagement-unity-roll-a-ball/38.png    
[51]: ./media/mobile-engagement-unity-roll-a-ball/new-project.png
[52]: ./media/mobile-engagement-unity-roll-a-ball/new-project-properties.png
[53]: ./media/mobile-engagement-unity-roll-a-ball/save-scene.png

    
    
    
    
    
    
    
    
    
    
    
    
