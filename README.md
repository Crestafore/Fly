# Fly!
Ова е наша имплементација на “Flappy Bird” со зголемен спектар на визуелни ефекти. Целта на играта е да се соберат што повеќе поени со цел да се надмине тековниот highscore. Птицата лета во околина исполнета со тунели меѓу кои треба да помине безбедно, а во исто време да собере што повеќе поени. 

<p align = "middle">
 <img src = "http://i.imgur.com/iSfkyaz.png" alt = "Playing Mode" />
</p>

Поените се собираат со поминување помеѓу тунелите(1 поен) и собирање на парички(2 поени). Повремо се појавува power-up со кој птицата преминува во empowered mode и самата си го наоѓа патот помеѓу тунелите во временски период од 4 секунди. Интеракцијата со птицата се прави со притискање на копчето “Space”. Времетраењето на empowered mode може да се види во десниот горен агол, додека поените се следат горе на средина. По истекувањето на empowered mode контролата на птицата се враќа на корисникот.

<p align = "middle">
 <img src = "http://i.imgur.com/eozbNQq.png" alt = "Empowered Mode" />
</p>

Во наштата имплементација користевме четири класи:

* Actor
  * Во класата Actor се чува позицијата на птицата, големината на птицата и во која состојба се наоѓа истата.
* Coin
  * Во класата Coin се чуваат информации за паричките.
* Tunnel
  * Во класата Tunnel се чуваат позицијата и големината на тунелите и видот на тунелот.
* Form1.cs
  * Во главната форма се чуваат две листи од класата Tunnel(topTerrain, bottomTerrain), листа од класата Coin и инстанца од класата Actor.

Една од поважните функции е функцијата gameOver() во која се проверува дали птицата се судрила со некој од тунелите (преклопување на два правоаголници) или излегла од границите на формата.

```C#
        public bool gameOver()
        {
            Point birdLeftTop = new Point(bird.X, bird.Y);
            Point birdRightBottom = new Point(bird.X + SQUARE_SIZE, bird.Y + SQUARE_SIZE - 12);

            if (birdRightBottom.Y >= MAX_HEIGHT) //check if u fell off the map
                return true;

            if (birdLeftTop.Y + SQUARE_SIZE < 0) //bitch overflew
                return true;

            if (bird.State != Actor.STATE.EXPIRED)
            {
                //check overlap of 2 rectangles
                foreach (Tunnel t in topTerrain)
                {
                    Point tunnelLeftTop = new Point(t.X, t.Y);
                    Point tunnelRightBottom = new Point(t.X + t.Width, t.Y + t.Height);
                    if (birdLeftTop.X < tunnelRightBottom.X && birdRightBottom.X > tunnelLeftTop.X && birdLeftTop.Y < tunnelRightBottom.Y && birdRightBottom.Y > tunnelLeftTop.Y)
                    {
                        gameOverSound.Play();
                        bird.State = Actor.STATE.EXPIRED;

                    }
                    //return true;
                }

                foreach (Tunnel t in bottomTerrain)
                {
                    Point tunnelLeftTop = new Point(t.X, t.Y);
                    Point tunnelRightBottom = new Point(t.X + t.Width, t.Y + t.Height);
                    if (birdLeftTop.X < tunnelRightBottom.X && birdRightBottom.X > tunnelLeftTop.X && birdLeftTop.Y < tunnelRightBottom.Y && birdRightBottom.Y > tunnelLeftTop.Y)
                    {
                        gameOverSound.Play();
                        bird.State = Actor.STATE.EXPIRED;

                    }
                    //return true;
                }
            }

            return false;
        }
```

При удирање на птицата со некој од тунелите состојбата на птицата се менува во EXPIRED и тогаш завршува играта и соодветната анимација се исцртува.

Почетниот екран има едноставен дизајн.

<p align = "middle">
 <img src = "http://i.imgur.com/M1YGdNm.png" alt = "Home Screen" />
</p>

Со клик на “Fly?” се почнува нова игра, а со клик на “Best” може да се види тековниот highscore.

<p align = "middle">
 <img src = "http://i.imgur.com/V6yen5E.png" alt = "Highscore" />
</p>

Кога ќе заврши играта има опција за враќање на почетната страна или да се исклучи играта.
