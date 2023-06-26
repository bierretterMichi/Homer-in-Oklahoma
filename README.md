# Homer in Oklahoma

    Ein Projekt von Michael Markwart

![Homer in Oklahoma](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/cb954616-c568-4a67-8515-bedc51fbcc99)

- [Einführung](#einf)
- [Wichtige Objekte](#konzept)
- [Blogeinträge](#blog)
- [Fazit](#fazit)
- [Eigenständigkeitserklärung](#eigenst)

## <a name="einf"></a>Einführung

In Oklahoma tobt das Chaos: Homer leidet unter einem gigantischen Heißhunger auf Donuts, die überall in der Stadt verstreut sind und von ihm eingesammelt werden müssen. Doch ein ständiger Gegenspieler steht ihm im Weg: der listige Burns. Als Millionär und Besitzer eines Atomkraftwerks versorgt er die Stadt mit Strom und Arbeitsplätzen, aber er ist entschlossen, Homer daran zu hindern und ihm die Donuts vor der Nase wegzuschnappen. Burns hat Millionen in ein Klonprojekt investiert, um sich selbst zu vervielfältigen und Homer zu bekämpfen. Gemeinsam mit seinem Doppelgänger wirft er Giftmüll aus dem Atomkraftwerk auf Homer, um ihn auszuschalten.

Hilf Homer dabei, sich gegen seinen Widersacher zu verteidigen und seinen Donut-Hunger zu stillen! Wie viele Donuts schaffst du zu essen? Tauche ein in das Abenteuer von Homer in Oklahoma und erlebe Spaß und Erfolg bei diesem spannenden Spiel!

Das ist: Homer in Oklahoma.

Wie viele Donuts schaffst du zu essen?


Viel Spaß und Erfolg!


## <a name="konzept"></a>Wichtige Objekte
In diesem Abschnitt werden Code und Funktion der wichtigsten Agenten des Spiels erklärt.

### Agenten
<details>
      <summary> Homer</summary>

![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/773da704-f7b1-4de0-914b-5aa9e68a5744)



```java 

    import greenfoot.*;

    public class Homer extends Actor {
    
    private int donutCounter = 0;
    
    private int damage = 3;
    private int damageTicks = 3;
    private int ticksSinceDamage = 0;
    private int tickCount = 0;                // Definition verschiedener Variablen wie z.B. Angriffsstärke, Gesundheit,
    private int health = 100;                 // Geschwindigkeit und ob er sich am Boden befindet
    private int maxHealth;
    
    private int vSpeed = 0;
    private boolean onGround = true;
    
    private AK47 ak47;
    
    
    public Homer(AK47 ak47) {      
        this.ak47 = ak47;
        this.health = health; 
        this.maxHealth = health;
    }
    
    public void act() {
        tastaturSteuerung();
        eatDonut();                            // Ausführung der Steuerung von Homer, Essen von Donuts, die Überprüfung der Kollision mit dem Crook-Akteur, 
        checkCollisionWithCrook();             // die Aktualisierung der Anzeige der Gesundheit, die Gravitation und die Zählung der Spielzeit
        updateHealthDisplay();
        gravity();
        tickCount++;
    }
    
    private void tastaturSteuerung() {
        if (Greenfoot.isKeyDown("a")) {
            if (isCarryingAK47()) {
                ak47.move(-4);
            }
            move(-4);
            ak47.setImage("ak47links.png");
        }
                
        if (Greenfoot.isKeyDown("d")) {
            if (isCarryingAK47()) {
                ak47.move(4);
            }
            move(4);
            ak47.setImage("ak47.png");                              //  Bewegung nach links ("a"), Bewegung nach rechts ("d"), Springen ("w") und Schießen ("space") 
        }                                                           //  und Wechsel zwischen der nach links/rechts ausgerichteten AK47.
        
        if (Greenfoot.isKeyDown("w") && onGround) {
            if (isCarryingAK47()) {
                vSpeed = -24;
                onGround = false;
            }
        }
        
        if (Greenfoot.isKeyDown("space") && isCarryingAK47()) {
            ak47.setShooting(true);
        } else {
            ak47.setShooting(false);
        }
    }
    
    private void gravity() {
        if (!onGround) {
            vSpeed = vSpeed + 1;
            setLocation(getX(), getY() + vSpeed);
            if (getY() >= getWorld().getHeight() - getImage().getHeight() / 2) {                  // Implementierung der Schwerkraft, durch Erhöhung der vertikalen Geschwindigkeit (vSpeed) von Homer 
                setLocation(getX(), getWorld().getHeight() - getImage().getHeight() / 2);         // und Bewegung nach unten, bis Homer wieder den Boden erreicht.
                vSpeed = 0;
                onGround = true;
            }
        }
    }
    
    private void eatDonut() {
    Donut donut = (Donut) getOneIntersectingObject(Donut.class);
    if (donut != null) {
        donut.removeDonut();                                              // Kollisionsüberprüfung zwischen Donut und Homer,
        getWorld().removeObject(donut);                                   // Entfernung vom Donut und Erhöung des Counters.
        donutCounter++;
        }
    }
       
    private void checkCollisionWithCrook() {
    Crook crook = (Crook) getOneIntersectingObject(Crook.class);
    if (crook != null && isTouching(Crook.class)) {                      
        if (tickCount % 10 == 0) {
            health -= damage;
        ticksSinceDamage++;
        }                                                                       //// Homer nimmt 10 Schaden auf 10 Gameticks verteilt, wenn er den Crook berührt.    

        if (ticksSinceDamage >= damageTicks) {
            if (health <= 0) {
                getWorld().showText("GAME OVER", getWorld().getWidth() / 2, getWorld().getHeight() / 2);
                Greenfoot.stop();
            }
            ticksSinceDamage = 0;
            }
        }
    }
   
    public void takeDamage(int damage) {
        health -= damage;
        if (health <= 0) {
        health = 0;
        if (health <= 0) {                                                                                  // Verringerung von Homers HP, Überprüfung, ob die 
            getWorld().showText("GAME OVER", getWorld().getWidth() / 2, getWorld().getHeight() / 2);        // Gesundheit null oder kleiner ist, um das Spiel zu beenden.
            Greenfoot.stop();
            }
        }
    }

    private void updateHealthDisplay() {
        World world = getWorld();

    
        String healthText = "Health: " + health;
        int fontSize = 24;
        Color fontColor = Color.BLACK;
        int xOffset = fontSize / 2 + 50;                              // Aktualisierung der HP-, und der Donutanzeige.
        int yOffset = fontSize / 2 + 10;                              
        world.showText(healthText, xOffset, yOffset);

        String donutText = "Donuts: " + donutCounter;
        int donutYOffset = yOffset + 60;
        world.showText(donutText, xOffset, donutYOffset);
    }

    public int getHealth() {
    return health;
    }

    public int getMaxHealth() {                      //aktuelle HP-Anzahl, maximaler HP-Wert und Festlegung des Gesundheitswertes.
    return maxHealth;
        }

    public void setHealth(int health) {
    this.health = health;
        }
    
     public void heal(int amount) {
        health += amount;
        if (health > maxHealth) {          // Erhöhung der HP und Verhinderung der Überschreitung der maximalen 100 HP.
            health = maxHealth;
        }
    }

    private boolean isCarryingAK47() {
        return ak47 != null;                  // Überprüfung, ob Homer die AK47-Class trägt, und gibt entsprechend true oder false zurück.
        }
    }

```

</details>
<details>    
<summary> Donut</summary>

![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/299309d0-53de-4406-8ae4-0350bc5f547f)


```java
  
    import greenfoot.*;

    public class Donut extends Actor {
    
    private static int counter = 0;                  
    private static final int HEAL_AMOUNT = 10;      // Um wie viel HP der Donut heilen soll.

    public Donut() {                                
        counter++;                                  //counter++ erhöht den Wert der Variablen "counter" jedes Mal, wenn ein neues Objekt Donut-Class erstellt wird.
    }

    public void act() {                         
        checkCollisionWithHomer();
    }

    public static int getNumberDonuts() {
        return counter;
    }

    public void removeDonut() {
        counter--;
    }

    public void healHomer(Homer homer) {
        int currentHealth = homer.getHealth();
        int newHealth = Math.min(currentHealth + HEAL_AMOUNT, homer.getMaxHealth());
        homer.setHealth(newHealth);                                                      // erhöht Homers HP, begrenzt sie auf die max. HP, und entfernt den Donut, wenn Homers HP erhöht wurde.
    
        if (newHealth > currentHealth) {
            getWorld().removeObject(this);
            counter--;
        }
    }

    public void checkCollisionWithHomer() {
        Homer homer = (Homer) getOneIntersectingObject(Homer.class);
        if (homer != null) {                                                            // Prüfung, ob Donut mit Homer kollidiert und Heilung für Homer, falls ja.
            healHomer(homer);
            }
        }
    }
```
</details>
<details>
  <summary> Crook</summary>

![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/b80cd224-b035-40ca-b901-d761bff55c4b)


```java

  
  import greenfoot.*;

    public class Crook extends Actor {
    private static int counter = 0;

    private Homer homer;
    private int throwCooldown = 200;                    // Selbsterklärend -> Steuerung des Cooldowns zum Werfen der Steine (Anpassbar für höheren Schwierigkeitsgrad).
    private int throwTimer = throwCooldown;

    public Crook(Homer homer) {
        this.homer = homer;
        counter++;
    }

    public void act() {
        moveTowardsHomer();                             // Während das Spiel ausgeführt wird, müssen diese drei Dinge in Dauerschleife laufen.
        checkCollision();
        throwStone();
    }

    public void moveTowardsHomer() {
        int xHomer = homer.getX();
        int yHomer = homer.getY();

        if (getX() < xHomer) {
            setLocation(getX() + 3/2, getY());
        } else if (getX() > xHomer) {
            setLocation(getX() - 3/2, getY());          // Das Programm checkt konstant, ob die Koordinaten von Homer größer oder kleiner sind, sodass die Crooks ihm durchgehend folgen können.
        }

        if (getY() < yHomer) {
            setLocation(getX(), getY() + 3/2);
        } else if (getY() > yHomer) {
            setLocation(getX(), getY() - 3/2);
        }
    }

    private void checkCollision() {
        Bullet bullet = (Bullet) getOneIntersectingObject(Bullet.class);
        if (bullet != null) {                                                  // Wenn eine Bullet einen Crook berührt, verschwindet dieser.
            getWorld().removeObject(bullet);
            removeCrook();
        }
    }

    private void throwStone() {
        if (throwTimer > 0) {
            throwTimer--;
        } else {
            int direction = getX() < homer.getX() ? 1 : -1;          // Erstellung neuer Steine, wenn der Timer nicht > 0 ist.
            Stone stone = new Stone(homer, direction);
            getWorld().addObject(stone, getX(), getY());
            throwTimer = throwCooldown;
        }
    }

    public static int getNumberCrooks() {
        return counter;
    }

    public void removeCrook() {
        counter--;
        getWorld().removeObject(this);
        }
    }

```

</details>
<details> 
<summary> Bullet</summary>

![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/edcbeb84-96fe-4149-baa5-eb708d2ccca6)


```java

    import greenfoot.*;

    public class Bullet extends Actor {
    
    public void act() {
        move(10);
        checkCollision();
    }
    
    private void checkCollision() {
        if (isTouching(Crook.class)) {
            Crook crook = (Crook) getOneIntersectingObject(Crook.class);
            crook.removeCrook();
            getWorld().removeObject(this);                                  // Prüfung auf eine Kollision zwischen Crook und Bullet.
        } else if (isAtEdge()) {
            getWorld().removeObject(this);
            }
        }
    }

```

</details>

<details>
<summary> AK47</summary>

![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/a691aefe-02ab-4631-ba5a-be94d927fe45)


```java

import greenfoot.*;

public class AK47 extends Actor {
    private int bulletSpeed = 5;
    private int bulletDelay = 25;                       // Schussverhalten
    private int bulletDelayCounter = bulletDelay;
    private boolean shooting = false;
    
    private boolean onGround = true;                    // Gravitation
    private int vSpeed = 0;
    
    private boolean isRotating = false;
    
    public void act() {
        checkKeyPress();
        checkBulletDelay();                        //  Tastatureingaben und Schusszeitverzögerung (je niedriger desto schneller wird geschossen)
        tastaturSteuerung();
        
        if (!onGround) {
            vSpeed = vSpeed + 1;
            setLocation(getX(), getY() + vSpeed);
            if (getY() >= getWorld().getHeight() - getImage().getHeight() / 2) {                      // Gravitation 
                setLocation(getX(), getWorld().getHeight() - 50 - getImage().getHeight() / 2);
                vSpeed = 0;
                onGround = true;
            }
        }
    }    
    
    private void tastaturSteuerung() {      
        if (Greenfoot.isKeyDown("w") && onGround) {
            vSpeed = -24;                                          // Sprung, weil homer und die AK47 zwei unterschiedliche Klassen sind.
            onGround = false;
        }
    }
    
    private void checkKeyPress() {
        if (Greenfoot.isKeyDown("space") && !shooting) {
            shooting = true;
        }
    }

    private void checkBulletDelay() {
        if (shooting) {
            if (bulletDelayCounter >= bulletDelay) {
                shoot();
                bulletDelayCounter = 0;                        // Wenn der bulletDelayCounter das bulletDelay überschreitet, darf man wieder schießen, 
            } else {                                           // andernfalls nimmt der bulletDelayCounter zu, bis man wieder schießen kann.
                bulletDelayCounter++;
            }
        }
    }

    private void shoot() {
        Bullet bullet = new Bullet();
        getWorld().addObject(bullet, getX(), getY());
        if (Greenfoot.isKeyDown("a")) {
            bullet.setRotation(180);
            bullet.move(-bulletSpeed);                          //  Bullet wird der Welt hinzugefügt und je nachdem, ob man "a" oder "d" drückt nach links oder rechts abgefeuert.
        }
        if (Greenfoot.isKeyDown("d")) {
            bullet.setRotation(0);
            bullet.move(bulletSpeed);
        }   
    }
    
    public boolean isShooting() {
        return shooting;
    }
    
    public void setShooting(boolean shooting) {
        this.shooting = shooting;
    }
}

```

</details>

<details>
  <summary> Stone</summary>

![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/eccdd4fd-33b0-49aa-81e6-400d8b9ef8ae)


```java

import greenfoot.*;

public class Stone extends Actor {
    private Homer homer;
    private int damage = 25;          // Eigenschaften des Stones
    private int speed = 5;
    private int direction;

    public Stone(Homer homer, int direction) {
        this.homer = homer;
        this.direction = direction;
    }

    public void act() {
        moveInDirection();
        checkCollision();
    }

    private void moveInDirection() {
        move(direction * speed);                // Damit der Stein sich nicht nur in eine Richtung bewegt, muss man ihm Homers Richtung relativ zu den Crooks geben.
    }

    private void checkCollision() {
        if (isTouching(Homer.class)) {
            homer.takeDamage(damage);
            getWorld().removeObject(this);        // takeDamage ist in Homer definiert, also nimmt Homer 25 Schaden pro Steinschlag, 
        } else if (isAtEdge()) {                  // welchen er jedoch ausweichen kann. Wenn er doch getroffen wird, verschwindet der Stein.
            getWorld().removeObject(this);
            }
        }
    }

```

</details>
 
<details>
  <summary> StartButton</summary>

![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/fca6916a-941b-4c77-ae86-c968b048501b)


```java

import greenfoot.*;

public class StartButton extends Actor {
    public void act() {
    if (Greenfoot.mouseClicked(this)) {
        StartScreen startScreen = (StartScreen) getWorld();        // Wechsel der Welt sobald man den Button anklickt.
        startScreen.startGame();
        startScreen.removeObject(this);
            }
        }
    }

```
  
</details>

### Welten

<details> 
<summary> StartScreen</summary>

![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/600fa64f-8a11-4ebd-92d9-da34ff3d37f1)


```java

import greenfoot.*;

    public class StartScreen extends World {
    public StartScreen() {
        super(1200, 600, 1);
        prepare();
    }

    private void prepare() {
        StartButton startButton = new StartButton();                // Größe der Welt, Erschaffung des StartButtons und Wechsel zur GameWorld
        addObject(startButton,600,200);
    }

    public void startGame() {
        Greenfoot.setWorld(new GameWorld());
        }
    }

```

  
</details>

<details> 
<summary> Gameworld</summary>

![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/cc24b5ea-d97c-47ba-9103-ae6fb0998aba)

```java

import greenfoot.*;

public class GameWorld extends World {
    
    private Homer homer;
    private AK47 ak47;
       
    public GameWorld() {
        super(1200, 600, 1);                  // Größe der Welt, Actors werden erschaffen.
        populateWorld();
    }
    
    public void act() {
        if (Donut.getNumberDonuts() < 1) {
            addObject(new Donut(), Greenfoot.getRandomNumber(1200) - Greenfoot.getRandomNumber(200), 400);          // Sobald <1 Donut auf der Welt ist, erscheinbt ein neuer. 
        }
        
        if (Crook.getNumberCrooks() <2) {
            addObject(new Crook(homer), Greenfoot.getRandomNumber(1200), 550);        // Sobald <2 Crooks auf der Welt sind, erscheint ein neuer.
        }
    }
    
    private void populateWorld() {
        ak47 = new AK47();
        homer = new Homer(ak47);
        Crook crook1 = new Crook(homer);                
        Crook crook2 = new Crook(homer);
                                                                                  // Erschaffung der Crooks, der AK47 und Homers
        addObject(homer, getWidth() / 2, getHeight() - 50);
        addObject(ak47, getWidth() / 2, getHeight() - 50);
        addObject(crook1, 300, getHeight() - 50);
        addObject(crook2, 900, getHeight() - 50);
    }
}


```

</details>

## <a name="blog"></a>Blogeinträge

| [14.02.2023](#1) | [28.02.2023](#2) | [14.03.2023](#3) | [21.03.2023](#4) | [04.04.2023](#5) |
| ------ | ------ | ------ | ------ | ------ |     
| [05.04.2023](#6) | [25.04.2023](#7) | [02.05.2023](#8) | [09.05.2023](#8) | [16.05.2023](#10) |
| [23.05.2023](#11) | [30.05.2023](#12) | [06.06.2023](#13) | [13.06.2023](#14) | [15.06.2023](#15) |


### <a name="1"></a>14.02.2023

In der ersten Stunde ging es erst einmal um die Projektfindung. Nachdem ich im letzten Programm zusammen mit Mohammad gearbeitet hatte, entschied ich mich, dieses Mal alleine zu arbeiten. Ich wollte unbedingt mehr in Richtung Textprogrammierung gehen, da diese sehr viel populärer ist und diese in mehr Bereichen Anwendung findet. Da das letzt Projekt "Homer in Ohio" hieß, entschied ich mich, eine "Fortsetzung" davon zu programmieren.
Zuerst wollte ich ein Dungeonexplorer ähnliches Spiel programmieren, bei dem Homer über Dächer läuft und verschiedene Gegner mit verschiedenen Waffen besiegt, aber entschied mich jedoch, nachdem mir dessen Kompliziertheit bewusst wurde, dagegen.

Die Fortsetzung nannte ich jedenfalls dann letztendlich "Homer in Oklahoma". In der ersten Stunde fing ich gar nicht erst an, zu programmieren, da ich mich erst mit Greenfoot vertraut machen wollte - und musste.
Als blutiger Neuling in der Textprogrammierung hatte ich keinerlei Erfahrung. Somit musste ich mich durch Tutorials, das Greenfoot-Buch und Szenarien auf der offiziellen Greenfoot-Website vertraut machen mit der Programmierweise vertraut machen.

### <a name="2"></a>28.02.2023

Bis zur nächsten Stunde waren schon zwei Wochen vergangen, in denen ich mich weiter in das Programm hinein arbeiten konnte. Zudem fing ich in der Zeit auch schon an den StartScreen, einen Startbutton und die Gameworld zu programmieren, welche ich in der Stunde am 28.02. weiter ausarbeitete. Diese drei Dinge stellten fast keine Probleme dar. Das einzige Problem, welches mich bis zuletzt meiner Arbeit mit dem Programm verfolgte, war die richtigen Commands zu finden, und diese zu verknüpfen. Die Commandfindung wurde zum Ende hin jedoch immer konstanter und schneller, was nicht zuletzt damit zu tun hat, dass Greenfoot ein Programm mit einer sehr steilen Lernkurve ist. Je mehr man programmiert, desto besser wird man.

<details>  
<summary> StartButton, StartScreen und GameWorld zum Zeitpunkt</summary>

  ![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/df6330ca-6ea3-487f-9656-91a5d09f569c)
  ![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/f00ac3c7-5de0-4b49-b741-fdb61f50e75f)
  ![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/1710cd4b-8bac-40e2-9dd9-1dc84af2b5e0)

Wie man sieht, habe ich auch schon Homer hinzugefügt, aber den Code für Homer habe ich erst in der nächsten Stunde angefangen.

</details>

### <a name="3"></a>14.03.2023

Bis 14.03.2023 waren schon wieder zwei Wochen vergangen. In der Zeit nahm ich mir vor, Homers Steuerung zu programmieren. der schwierige Teil bestand hier jedoch nicht in der "w", "a", "d" Steuerung, sondern darin, die Gravitation zu programmieren.

<details>
<summary> Steuerung und Gravitation</summary>

![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/315a00fc-00f3-4d66-9201-2182a1aad5de)

In diesem Screenshot sieht man, wie unorganisiert ich war. Dass man Funktionen im public void act Feld nicht ausschreibt, war mir hier noch nicht bekannt.
  
</details>

Um die Gravitation zu programmieren, ging ich eine Menge Szenarien auf der Greenfoot-Website (https://www.greenfoot.org/scenarios) durch und suchte nach Inspirationen. Ich versuchte, so wenig wie möglich 1:1 zu kopieren, sondern den Code zu verstehen, und demnach meinen eigenen zu schreiben.


### <a name="4"></a>21.03.2023

Am 21.03. fügte ich die Donut Klasse hinzu. Donuts sind dazu gedacht, den Fortschritt im Spiel zu messen. Durch das Einsammeln soll man Highscores erzielen und sich evtl. mit anderen Spielern messen können.

<details> 
<summary> Donut</summary>

![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/027596dd-576e-4246-b32a-a8b9ea9738ec)

Wie man sieht, ist der Code hier schon recht ähnlich zum finalen Produkt. Es fehlt nurnoch die "healHomer" Methode, aber das Prinzip ist erkennbar.
  
</details>

Für die nächste Stunde nahm ich mir das Ziel Gegner zu erstellen. Da Burns schon fies aussieht und ebenfalls aus "Die Simpsons" kommt, war die Wahl schnell auf ihn gefallen.

### <a name="5"></a>04.04.2023

Am 04.04. überlegte ich, wie man Crooks im Spiel implementieren könnte. Einmal hat man natürlich die Möglichkeit, dass sie Homer schaden können. Außerdem überlegte ich mir, wie die anderen, bereits vorhandenen, Agenten mit ihnen interagieren können. Beispielsweise, dass ich Waffen für Homer erstelle, mit deren Hilfe er sich verteidigen kann. 

Also fügte ich eine Crook Klasse hinzu. Diese sah zum Zeitpunkt wie folgt aus.

<details> 
<summary> Crook</summary>

  ![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/816b0306-a813-4757-95c1-c52ee29a0ced)

Natürlich hatte ich viele Fehler und unsinniges zwischendurch im Code, was ich jedoch nicht dokumentiert habe. Beispielsweise versuchte ich, dass der Crook sich nicht direkt, auf Homer zubewegt, also in einem zufälligen Zick-Zack auf ihn zubewegt, was mir mislungen war. Somit blieb ich mit dieser Lösung.
</details>

### <a name="6"></a>05.04.2023

Der 05.04. war ein Informatikfachtag, an welchem ich leider nicht teilnehmen konnte, weil es mir nicht gut ging. Ich versuchte trotzdem so gut es ging, von zuhause aus zu arbeiten, und führte den Code vom Crook weiter aus.


### <a name="7"></a>25.04.2023

Über Die Ferien hinweg fügte ich eine neue Klasse hinzu. Ich fand nämlich die Bewegung der Crooks alleine ziemlich langweilig. Und da Burns ein Kernkraftwerk besitzt, würde es Sinn machen, wenn er mit Giftmüll wirft. Und es wäre auch ziemlich lustig. Also entschied ich mich die Klasse "Stone" zu erstellen.

<details> 
<summary>  Stone</summary>

  ![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/34cc08a7-79c6-41b3-bdb9-3ef88d8e6987)

Der finale Code, den man hier sieht, setzte sich aus Richtungsbewegung, aber auch aus einer Damagefunktion zusammen. Die Damagefunktion habe ich jedoch erst später programmiert.
Also ging es mir zu diesem Zeitpunkt eher um die Bewegung selbst, mit welcher ich eine Menge herumexperimentierte. Ich wollte sie so einstellen, dass Homer immernoch ausweichen konnte, was sich als äußerst schwierig herausstellte. Zum Beispiel wollte ich Gravitation und eine Flugbahn für den Stein hinzufügen, was ich jedoch aufgab. Falls man es im Spiel jedoch schwieriger haben will, kann man, wie ich vorhin schon in der Erklärung der Gruppen erwähnte, den Schaden erhöhen, sodass man noch härter bestraft wird, falls man getroffen wird.

</details>

### <a name="8"></a>02.05.2023 & 09.05.2023

Um diese Zeit herum realisierte ich, dass die Crooks Homer abschießen können - aber warum nicht anders herum? Jeder mag es, "einfach mal draufloszuballern".
Zudem meinte Rosalie zu mir: "Füg doch eine AK47 hinzu!" Und genau das tat ich dann. Das ist einer der Vorteile, wenn man mit Greenfoot programmiert - wenn man einmal eine neue Idee hat, kann man sie ohne großen Aufwand hinzufügen.

Also lag mein Fokus nun darauf, die AK47 Klasse zu programmieren. 
<details>  
  <summary>  AK47</summary>

  ![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/89468eb3-fab3-4661-9628-5d4d29ed9c5a)

Wie man in diesem Code sieht, habe ich die Gravitation aus der Homer Klasse übernommen. Dies liegt daran, dass Homer und AK47 offensichtlich nicht dieselbe Klasse sind, und wenn es so aussehen soll, als träge Homer die AK47, braucht sie natürlich dieselbe Steuerung, wie Homer sie hat.
Das war es soweit auch für die AK47 selbst, denn was ist eine AK47 ohne Munition?
Also setzte ich mich gleich in der nächsten Stunde an die Bullet Klasse. 
</details>

### <a name="10"></a>16.05.2023


Um die Bullet Klasse selbst zu programmieren, brauchte ich nicht viel: ein Movement und einen Kollisionscheck mit den Crooks, da diese mit einem Schuss getötet werden können sollten.

<details>
<summary> Bullet</summary>

  ![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/a92f0d97-ea5a-445c-9b54-9a8030e0e49a)


</details>

### <a name="11"></a>23.05.2023

Die Stunde am 23.05. verbrachte ich damit, die Bulletfunktionen, wozu das Schießen und ein Schussdelay gehören, in die AK47 Klasse einzubauen.

<details> 
<summary> Bulletfunktionen in AK47</summary>

  ![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/b392e277-fd33-40c8-8d05-6fbf919a7579)

Je nachdem wie man den Schwierigkeitsgrad einstellen will kann man auch das Bulletdelay hochstellen, sodass die AK47 langsamer schießt. Somit ist man gezwungen, auf richtige Momente zu warten und nicht zu voreilig zu handeln, da wenn man Crooks abschießt, sie immer zufällig wieder spawnen und man schnell eingekesselt werden kann. (siehe unterer Screenshot)

  ![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/1e7d1b5c-a50a-40e8-962a-6ef7776a877b)


</details>

### <a name="12"></a>30.05.2023

Am 30.05. stellte ich zunächst die AK47 und die Bullet Klasse zuende. Dann fing ich an, mir Gedanken zu machen, wie ich HP, also HealthPoints für Homer implementieren könnte - und wie ich sie mit dem Schaden der Crooks ausbalanciere, denn das Spiel darf nicht zu einfach, aber auch nicht zu schwer werden.

<details>
<summary> HealthPoints</summary>

Ich fügte also erst eine Funktion namens "takeDamage" hinzu, welche ich dann in der Stone Klasse verwenden konnte.
<details> 
<summary> Kollision in der Stone Klasse</summary>

  ![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/1ab58000-1234-45fa-97a5-d0b9517f18be)

</details>



<details> 
<summary> Code zum Nachlesen</summary>


```java

import greenfoot.*;

public class Homer extends Actor {
    
    private int donutCounter = 0;
    
    private int damage = 3;
    private int damageTicks = 3;
    private int ticksSinceDamage = 0;
    private int tickCount = 0;
    private int health = 100;
    private int maxHealth;
    
    private int vSpeed = 0;
    private boolean onGround = true;
    
    private AK47 ak47;
    
    
    public Homer(AK47 ak47) {
        this.ak47 = ak47;
        this.health = health; 
        this.maxHealth = health;
    }
    
    public void act() {
        tastaturSteuerung();
        eatDonut();
        checkCollisionWithCrook();
        updateHealthDisplay();
        gravity();
        tickCount++;
    }
    
    private void tastaturSteuerung() {
        if (Greenfoot.isKeyDown("a")) {
            if (isCarryingAK47()) {
                ak47.move(-4);
            }
            move(-4);
            ak47.setImage("ak47links.png");
        }
                
        if (Greenfoot.isKeyDown("d")) {
            if (isCarryingAK47()) {
                ak47.move(4);
            }
            move(4);
            ak47.setImage("ak47.png");
        }
        
        if (Greenfoot.isKeyDown("w") && onGround) {
            if (isCarryingAK47()) {
                vSpeed = -24;
                onGround = false;
            }
        }
        
        if (Greenfoot.isKeyDown("space") && isCarryingAK47()) {
            ak47.setShooting(true);
        } else {
            ak47.setShooting(false);
        }
    }
    
    private void gravity() {
        if (!onGround) {
            vSpeed = vSpeed + 1;
            setLocation(getX(), getY() + vSpeed);
            if (getY() >= getWorld().getHeight() - getImage().getHeight() / 2) {
                setLocation(getX(), getWorld().getHeight() - getImage().getHeight() / 2);
                vSpeed = 0;
                onGround = true;
            }
        }
    }
    
    private void eatDonut() {
        Donut donut = (Donut) getOneIntersectingObject(Donut.class);
        if (donut != null) {
            donut.removeDonut();
            getWorld().removeObject(donut);
            donutCounter++;
        }
    }
       
    private void checkCollisionWithCrook() {
        Crook crook = (Crook) getOneIntersectingObject(Crook.class);
        if (crook != null && isTouching(Crook.class)) {
            if (tickCount % 10 == 0) {
                health -= damage;
            ticksSinceDamage++;
            }

            if (ticksSinceDamage >= damageTicks) {
                if (health <= 0) {
                    getWorld().showText("GAME OVER", getWorld().getWidth() / 2, getWorld().getHeight() / 2);
                    Greenfoot.stop();
                }
                ticksSinceDamage = 0;
            }
        }
    }
   
    public void takeDamage(int damage) {
        health -= damage;
        if (health <= 0) {
        health = 0;
        if (health <= 0) {
            getWorld().showText("GAME OVER", getWorld().getWidth() / 2, getWorld().getHeight() / 2);
            Greenfoot.stop();
            }
        }
    }

    private void updateHealthDisplay() {
        World world = getWorld();

    
        String healthText = "Health: " + health;
        int fontSize = 24;
        Color fontColor = Color.BLACK;
        int xOffset = fontSize / 2 + 50;
        int yOffset = fontSize / 2 + 10;
        world.showText(healthText, xOffset, yOffset);

        String donutText = "Donuts: " + donutCounter;
        int donutYOffset = yOffset + 60;
        world.showText(donutText, xOffset, donutYOffset);
    }

    public int getHealth() {
    return health;
    }

    public int getMaxHealth() {
    return maxHealth;
        }

    public void setHealth(int health) {
    this.health = health;
        }
    
     public void heal(int amount) {
        health += amount;
        if (health > maxHealth) {
            health = maxHealth;
        }
    }

    private boolean isCarryingAK47() {
        return ak47 != null;
    }
}

```
</details>

  
</details>

### <a name="13"></a>06.06.2023

Ich nutzte die Stunde, um Homer damage zuzufügen, wenn er mit einem Crook kollidiert.

<details>
<summary> Crook Kollision</summary>

![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/9e0890fe-10fb-4fed-8f6a-2ca6c0a19b9a)

Dies erreichte ich, indem der Crook 10 Schaden zufügt, welche zur Anschaulichkeit über 1000ms über jeweils 1 Schaden pro 100ms verteilt werden.
Zudem fügte ich hinzu, dass das Spiel vorbei ist, sobald Homer 0 HP erreicht.

Die HP werden, wie die Donuts, in einem Counter veranschaulicht.

<details>
<summary> Code der Counter </summary>

  ![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/bbf93462-865f-4ad3-82dd-e527f7ec7393)
</details>

<details>
<summary> Bild der Counter im Spiel</summary>

  ![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/c7fccb49-9d28-473e-9192-8f73e9bc62f9)

</details>



</details>

### <a name="14"></a>13.06.2023

Damit Homer geheilt werden kann, definierte ich nun "healHomer" in der DonutKlasse, was ich vorher, wie gesagt, nicht getan hatte. "healHomer" Konnte ich dann in die Kollisionsfunktion zwischen Homer und Donut einbauen.

<details> 
<summary> Heilung für Homer durch die Donutkollision</summary>

  ![image](https://github.com/bierretterMichi/Homer-in-Oklahoma/assets/111492177/f71e37ae-e467-4480-9b55-f7020184f734)

</details>

### <a name="15"></a>15.06.2023


Den zweiten Fachtag, sowie die restliche Zeit nutzte ich, um allerlei Feinschliffe zu fertigen und meine Github Seite zu vervollständigen.



## <a name="fazit"></a>Fazit

Die Arbeit mit Greenfoot und die Entwicklung dieses Projekts war eine spannende und lehrreiche Erfahrung. Als Greenfoot-Neuling musste ich mich zunächst mit den Grundlagen der Plattform vertraut machen, um die Möglichkeiten und Konzepte zu verstehen. Es war interessant zu sehen, wie Greenfoot die Entwicklung von 2D-Spielen vereinfacht und anschauliches Programmieren ermöglicht.
Das Projekt selbst, "Homer in Oklahoma", hat mir die Möglichkeit gegeben, verschiedene Aspekte der Spielentwicklung zu erkunden. Ich musste verschiedene Klassen erstellen, um die Spielobjekte zu repräsentieren, Kollisionen zu behandeln, den Spieler zu steuern und vieles mehr. Die Herausforderungen beim Implementieren der Spiellogik und das Lösen von Problemen haben mir geholfen, meine Programmierkenntnisse zu verbessern und meine Fähigkeiten in der Fehlerbehebung zu schärfen.

Insgesamt war es eine aufregende Zeit, bei der ich nicht nur die Grundlagen von Greenfoot erlernt, sondern auch ein lustiges Spiel entwickelt habe.

## <a name="eigenst"></a>Eigenständigkeitserklärung

Hiermit bestätige ich, Michael Markwart, dass die von mir vorgelegte Arbeit selbstständig verfasst wurde und keine anderen als die angegebenen Quellen und Hilfsmittel benutzt wurden.


Ahrensburg, den 26.06.2022
