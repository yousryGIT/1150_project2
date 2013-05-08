1150_project2
=============



<!DOCTYPE HTML>
<html>
    <head>
        <title>Wezza Invaders</title>
        <style>
            body {
                margin-top: 0;
                margin-bottom: 0;
                margin-left: 300px;
                margin-right: 300px;
                padding: 0;
                background:url('5.jpg') no-repeat;
                background-color: #000000;
            }
            #Pause{
                position: absolute;
                top: 0;
                left: 600px;
            }
            #exit{
                position: absolute;
                top: 0;
                left: 640px;
            }
            #newgame{
                position: absolute;
                top: 0;
                left: 690px;
            }
        </style>
        <script src="pixi.js"></script>
        <script src="WebGLRenderer.js"></script>

    </head>
    <body>
        <div id="Pause">
            <img  src="pause.png" alt="pause" height="30" width="30" onclick="Pause();">
        </div>
        <div id="exit">
            <img  src="close.gif" alt="exit" height="30" width="40" onclick="exit();">
        </div>
        <div id="newgame">
            <img  src="newgame.jpg" alt="newgame" height="30" width="30" onclick="newgame();">
        </div>
    <audio controls>
        <source src="audio.mp3" type="audio/mpeg">
    </audio>
    <label id="Scorelabel" title="Score: " style="position:fixed;color:white;font-size:30px;visibility:hidden;margin-left:0;margin-top:50px;"></label>
    <label id="Score" title="0" style="position:fixed;color:white;font-size:30px;visibility:visible;margin-left:390px;margin-top:50px;"></label>
    <script>

        var flag_Move = true;
        var flag_CreateAliens = true;
        var flag_Bullet = true;
        var pause = 1;
        

        document.getElementById('Scorelabel').style.visibility="visible";
        document.getElementById('Scorelabel').innerHTML="Score: ";
        document.getElementById('Score').innerHTML="0";
        var Score = 0;
        var Lifes = 3;
        var stage = new PIXI.Stage(0x000000, true);
        var renderer = PIXI.autoDetectRenderer(window.innerWidth-600, window.innerHeight);
        document.body.appendChild(renderer.view);


        function Pause(){
            if(pause == 1){
                flag_Move = false;
                flag_CreateAliens = false;
                flag_Bullet = false;
                pause = 0 ;
            }else{
                flag_Move = true;
                flag_CreateAliens = true;
                flag_Bullet = true;
                pause = 1;
            }
        }

        function exit()
        {
            document.body.removeChild(renderer.view);
            document.getElementById('Scorelabel').style.visibility="hidden";
            document.getElementById('Score').style.visibility="hidden";

        }

        function newgame()
        {

            document.body.removeChild(renderer.view);
            document.body.appendChild(renderer.view);
            requestAnimFrame( animate );
            var backtexture = PIXI.Texture.fromImage("background.jpg");
            var background = new PIXI.Sprite(backtexture);
            background.anchor.x = 0.5;
            background.anchor.y = 0.5;
            background.position.x = (window.innerWidth-600)/2;
            background.position.y = (window.innerHeight/2);
            stage.addChild(background);

            var backtexture2 = PIXI.Texture.fromImage("background1.jpg");
            var background2 = new PIXI.Sprite(backtexture2);
            background2.anchor.x = 0.5;
            background2.anchor.y = 0.5;
            background2.position.x = (window.innerWidth-600)/2;
            background2.position.y = -(1200+600);
            stage.addChild(background2);

            var life1texture = PIXI.Texture.fromImage("shooter1.png");
            var life1 = new PIXI.Sprite(life1texture);
            life1.position.x = ((window.innerWidth-600)/2)+220;
            life1.position.y = ((window.innerHeight)/2)-230;
            life1.anchor.x = 0.5;
            life1.anchor.y = 0.5;
            life1.scale.x = 0.4;
            life1.scale.y = 0.4;
            stage.addChild(life1);

            var life2texture = PIXI.Texture.fromImage("shooter1.png");
            var life2 = new PIXI.Sprite(life2texture);
            life2.position.x = ((window.innerWidth-600)/2)+220;
            life2.position.y = ((window.innerHeight)/2)-190;
            life2.anchor.x = 0.5;
            life2.anchor.y = 0.5;
            life2.scale.x = 0.4;
            life2.scale.y = 0.4;
            stage.addChild(life2);


            var life3texture = PIXI.Texture.fromImage("shooter1.png");
            var life3 = new PIXI.Sprite(life3texture);
            life3.position.x = ((window.innerWidth-600)/2)+220;
            life3.position.y = ((window.innerHeight)/2)-150;
            life3.anchor.x = 0.5;
            life3.anchor.y = 0.5;
            life3.scale.x = 0.4;
            life3.scale.y = 0.4;
            stage.addChild(life3);

            var aliens = [];
            var aliens2 = [];
            var alientexture = PIXI.Texture.fromImage("alien1.png");
            var alientexture2 = PIXI.Texture.fromImage("alien2.png");

            function createAlien(x, y)
            {
                if(flag_CreateAliens){
                    var alien = new PIXI.Sprite(alientexture);
                    alien.anchor.x = 0.5;
                    alien.anchor.y = 0.5;
                    alien.position.x = x;
                    alien.position.y = y;
                    alien.scale.x = 0.5;
                    alien.scale.y = 0.5;
                    aliens.push(alien);
                    stage.addChild(alien);
                    if(Score >= 1000)
                    {

                        var alien = new PIXI.Sprite(alientexture);
                        alien.anchor.x = 0.5;
                        alien.anchor.y = 0.5;
                        alien.position.x = x;
                        alien.position.y = y;
                        alien.scale.x = 0.5;
                        alien.scale.y = 0.5;
                        aliens.push(alien);
                        stage.addChild(alien);

                        var alien2 = new PIXI.Sprite(alientexture2);
                        alien2.anchor.x = 0.5;
                        alien2.anchor.y = 0.5;
                        alien2.position.x = Math.random() * (window.innerWidth-600);
                        alien2.position.y = y;
                        alien2.scale.x = 0.2;
                        alien2.scale.y = 0.2;
                        aliens2.push(alien2);
                        stage.addChild(alien2);

                    }
                    if(Score % 3000 == 0)
                    {
                        for(var i=0 ; i<aliens2.length ; i++){

                            var Monster_A = aliens2[i];
                            aliens2.splice(i, 1);
                            stage.removeChild(Monster_A);

                        }

                    }
                }
            }

            function movingAliens()
            {
                if(flag_Move){
                    for (var i = 0; i < aliens.length ; i++)
                    {
                        var alien = aliens[i];
                        alien.position.y += 10;
                    }
                    if(Score >= 1000)
                    {
                        for (var i = 0; i < aliens.length ; i++)
                        {
                            var alien = aliens[i];
                            var alien2 = aliens2[i];
                            alien.position.y += 1;
                            alien2.position.y += 1;
                        }
                    }
                }
            }


            var shootertexture = PIXI.Texture.fromImage("shooter1.png");
            var shooter = new PIXI.Sprite(shootertexture);
            shooter.anchor.x = 0.5;
            shooter.anchor.y = 0.5;
            shooter.position.x = (window.innerWidth-600)/2;
            shooter.position.y = (window.innerHeight/2);
            shooter.scale.x = 0.8;
            shooter.scale.y = 0.8;


            shooter.setInteractive(true);

            shooter.mousemove = function(data)
            {
                if(flag_Move){
                    var newPosition = this.data.getLocalPosition(this.parent);
                    shooter.position.x = newPosition.x;
                    shooter.position.y = newPosition.y;
                }

            }

            var Bullets = [];
            shooter.mousedown = shooter.touchstart =  function(data)
            {
                this.data = data;
                this.alpha = 0.9;
                //this.dragging = true;
                if(flag_Bullet){
                    var BulletTexture = PIXI.Texture.fromImage("bunny.png");
                    var Bullet = new PIXI.Sprite(BulletTexture);
                    Bullet.position.x = shooter.position.x - 10;
                    Bullet.position.y = shooter.position.y - 50;
                    Bullets.push(Bullet);
                    stage.addChild(Bullet);
                }
            }



            stage.addChild(shooter);

            function movingBullets()
            {
                for (var i = 0; i < Bullets.length ; i++)
                {
                    var bullet = Bullets[i];
                    bullet.position.y -= 10;
                }
            }

            // create an array of assets to load
            var assetsToLoader = [ "SpriteSheet.json"];

            // create a new loader
            loader = new PIXI.AssetLoader(assetsToLoader);

            // use callback
            loader.onComplete = onAssetsLoaded

            //begin load
            loader.load();

            var explosion;


            function onAssetsLoaded(x,y)
            {
                // create an array to store the textures
                var explosionTextures = [];

                for (var i=0; i < 26; i++)
                {
                    var texture = PIXI.Texture.fromFrame("Explosion_Sequence_A " + (i+1) + ".png");
                    explosionTextures.push(texture);
                }

                // create a texture from an image path
                // add a bunch of aliens

                // create an explosion MovieClip
                explosion = new PIXI.MovieClip(explosionTextures);


                explosion.position.x = x;
                explosion.position.y = y;
                explosion.anchor.x = 0.5;
                explosion.anchor.y = 0.5;



                explosion.gotoAndPlay(1);

                stage.addChild(explosion);




            }
            function CollisionWithBullet()
            {
                for(var i = 0;i< aliens.length;i++){
                    for(var j = 0;j< Bullets.length;j++){
                        var Bullet_A = Bullets[j];
                        var Monster_A = aliens[i];

                        if((Bullet_A.position.x < (Monster_A.position.x + Monster_A.width)) && ((Bullet_A.position.x + Bullet_A.width) > Monster_A.position.x) && (Bullet_A.position.y < (Monster_A.position.y + Monster_A.height)) && ((Bullet_A.position.y + Bullet_A.height) > Monster_A.position.y)){

                            Score += 20;
                            document.getElementById('Score').innerHTML=Score;
                            aliens.splice(i, 1);
                            Bullets.splice(j, 1);
                            stage.removeChild(Bullet_A);
                            stage.removeChild(Monster_A);

                            onAssetsLoaded(((Bullet_A.position.x)+(Monster_A.position.x))/2,((Bullet_A.position.y)+(Monster_A.position.y))/2);

                        }
                    }
                }
            }

            function CollisionWithShooter()
            {
                for(var i=0 ; i<aliens.length ; i++){
                    var Monster_A = aliens[i];
                    if((shooter.position.x < (Monster_A.position.x + Monster_A.width)) && ((shooter.position.x + shooter.width) > Monster_A.position.x) && (shooter.position.y < (Monster_A.position.y + Monster_A.height)) && ((shooter.position.y + shooter.height) > Monster_A.position.y))
                    {

                        if(Lifes == 3)
                        {
                            stage.removeChild(life3);
                            aliens.splice(i, 1);
                            stage.removeChild(Monster_A);
                            onAssetsLoaded(((shooter.position.x)+(Monster_A.position.x))/2,((shooter.position.y)+(Monster_A.position.y))/2);
                        }else if(Lifes == 2)
                        {
                            stage.removeChild(life2);
                            aliens.splice(i, 1);
                            stage.removeChild(Monster_A);
                            onAssetsLoaded(((shooter.position.x)+(Monster_A.position.x))/2,((shooter.position.y)+(Monster_A.position.y))/2);
                        }
                        else if(Lifes == 1)
                        {
                            stage.removeChild(life1);
                            stage.removeChild(shooter);
                            onAssetsLoaded(((shooter.position.x)+(Monster_A.position.x))/2,((shooter.position.y)+(Monster_A.position.y))/2);
                            var gotexture = PIXI.Texture.fromImage("gameover.png");
                            var go = new PIXI.Sprite(gotexture);
                            go.anchor.x = 0.5;
                            go.anchor.y = 0.5;
                            go.position.x = (window.innerWidth-600)/2;
                            go.position.y = (window.innerHeight/2);
                            go.scale.x = 0.8;
                            go.scale.y = 0.8;
                            stage.addChild(go);

                            flag_Move = false;
                            flag_CreateAliens = false;
                            flag_Bullet = false;
                        }

                        Lifes -= 1;
                    }
                }
            }

            //LEVEL 2
            function CollisionWithShooter2()
            {
                for(var i=0 ; i<aliens2.length ; i++){
                    var Monster_A = aliens2[i];
                    if((shooter.position.x < (Monster_A.position.x + Monster_A.width)) && ((shooter.position.x + shooter.width) > Monster_A.position.x) && (shooter.position.y < (Monster_A.position.y + Monster_A.height)) && ((shooter.position.y + shooter.height) > Monster_A.position.y))
                    {

                        if(Lifes == 3)
                        {
                            stage.removeChild(life3);
                            aliens2.splice(i, 1);
                            stage.removeChild(Monster_A);
                            onAssetsLoaded(((shooter.position.x)+(Monster_A.position.x))/2,((shooter.position.y)+(Monster_A.position.y))/2);
                        }else if(Lifes == 2)
                        {
                            stage.removeChild(life2);
                            aliens2.splice(i, 1);
                            stage.removeChild(Monster_A);
                            onAssetsLoaded(((shooter.position.x)+(Monster_A.position.x))/2,((shooter.position.y)+(Monster_A.position.y))/2);
                        }
                        else if(Lifes == 1)
                        {
                            stage.removeChild(life1);
                            stage.removeChild(shooter);
                            onAssetsLoaded(((shooter.position.x)+(Monster_A.position.x))/2,((shooter.position.y)+(Monster_A.position.y))/2);
                            var gotexture = PIXI.Texture.fromImage("gameover.png");
                            var go = new PIXI.Sprite(gotexture);
                            go.anchor.x = 0.5;
                            go.anchor.y = 0.5;
                            go.position.x = (window.innerWidth-600)/2;
                            go.position.y = (window.innerHeight/2);
                            go.scale.x = 0.8;
                            go.scale.y = 0.8;
                            stage.addChild(go);

                            flag_Move = false;
                            flag_CreateAliens = false;
                            flag_Bullet = false;


                        }

                        Lifes -= 1;
                    }
                }
            }




            var count = 10;
            var mcount = 10;
            var skip= 0;

            function action()
            {

                count -= 10;
                mcount += 10;

                skip +=1;
                if(skip % 40 ==0)
                {
                    createAlien(Math.random() * (window.innerWidth-600), -20);
                    movingAliens();
                }
                movingBullets();
                CollisionWithBullet();
                CollisionWithShooter();
                if(Score >= 1000)
                {
                    CollisionWithShooter2();
                }

                if(flag_Move){
                    background.position.y += 10;
                    background2.position.y += 10;
                    if(background.position.y >= (1200+600))
                    {
                        background.position.y = -(1200);
                    }
                    if(background2.position.y >= (1200+600))
                    {
                        background2.position.y = -(1200);
                    }
                }
            }


            function animate()
            {

                requestAnimFrame( animate );
                action();
                renderer.render(stage);
                setInterval(explosion.gotoAndStop(25), 1000);
            }
        }

    </script>

</body>
</html>
