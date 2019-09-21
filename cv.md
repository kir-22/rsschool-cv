# KIREYEU KIRYLL

## CONTACTS:
* **Phone:** _+375-29-809-24-90_
* **E-mail:** _kirill89kireew@gmail.com_
* **Skype:** _kirill.kireev9_

## SUMMARY:
> I learn easily. I like to learn something new. I'am purposeful, responsible. I like Javascript. I'd like to progress in Webdevelopment and create beautiful applications. I study Javascript and his frameworks (Node.js, TypeScript) and library React.js himself. I finish a course - "Develop Game on Unity3D". I also want to create a HTML5 game. And I will make every effort to become a FrontEnd developer!

## SKILLS:
* C#(for Unity3D), JavaScript, HTML5, CSS3, NodeJS(basic), Unity3D, TypeScript, Less, Photoshop, Psdetch, Git, Bitrix, React(junior), React Native(basic), Webpack, Gulp.

## EXAMPLE CODE:
**Links on project**
* [CV](https://kir-22.github.io/rsschool-codejam1-cv/) - My first CV
* [Youtube](https://kir-22.github.io/youtube-client/) - Find the video in YouTube
* [Dashboard](https://kir-22.github.io/dashboard/) - 3 Stage tasks
* [Game](https://kir-22.github.io/MonsterGame/) - My First Game in team
* [Presentation](https://kir-22.github.io/TypeScript/index.html) - Presentation about TypeScript on English
* [Lading](https://kir-22.github.io/markup-2018q3/) - Lading Page

**Example fetch response of Youtube task**

    import { init, addInit } from './init';
    let next;
    let prev;
    export{next, prev};

    export function ajax(){

     fetch(`https://www.googleapis.com/youtube/v3/search?key=AIzaSyDR4nYtKG6R4r2ByKEmvGl9Q3ulSRJcHbM&type=video&part=snippet&maxResults=15&q=${this.value}`)

    .then((response)=>{
      return response.json();
    })

      .then((data)=>{
        next = data.nextPageToken;
        let arr = data.items.map((item)=> item.id.videoId);

        fetch(`https://www.googleapis.com/youtube/v3/videos?key=AIzaSyCTWC75i70moJLzyNh3tt4jzCljZcRkU8Y&id=${arr.join(',')}&part=snippet,statistics`)
        .then((res)=>{
          return res.json();
        })

        .then((stat)=>{
          init(stat.items);
        })
    });

    }

    export function addedAjax(next){
        fetch(`https://www.googleapis.com/youtube/v3/search?key=AIzaSyDR4nYtKG6R4r2ByKEmvGl9Q3ulSRJcHbM&type=video&part=snippet&maxResults=15&pageToken=${next}`)
        .then((response)=>{
          return response.json();
        })
        .then((data)=>{
          next = data.nextPageToken;
          prev = data.prevPageToken;
          let arr = data.items.map((item)=> item.id.videoId);
          fetch(`https://www.googleapis.com/youtube/v3/videos?key=AIzaSyCTWC75i70moJLzyNh3tt4jzCljZcRkU8Y&id=${arr.join(',')}&part=snippet,statistics`)
          .then((res)=>{
            return res.json();
          })
          .then((stat)=>{
            addInit(stat.items);
          })
        });
    }
***

**My Game on HTM5 - Arcanoud**

    const game = {
                  width:640,
                  height: 360,
                  ctx: undefined,
                  platform:undefined,
                  ball:undefined,
                  rows: 4,
                  cols:8,
                  blocks:[],
                  score:0,
                  running: true,
                  sprites:{
                      background: undefined,
                      platform: undefined,
                      ball: undefined,
                      block:undefined
                  },
                  init: function(){
                      var canvas=document.getElementById('mycanvas');
                      this.ctx=canvas.getContext('2d');
                      this.ctx.font='20px Arial';
                      this.ctx.fillStyle='#FFFFFF';
              
                      window.addEventListener('keydown',function(e){
                          if(e.keyCode == 37){
                              game.platform.dx = -game.platform.speed;
                          }else if(e.keyCode == 39){
                              game.platform.dx=game.platform.speed;
                          }else if(e.keyCode == 32){
                              game.platform.releaseBall();
                              
                          }
                      });
                      window.addEventListener('keyup',function(e){
                          game.platform.stop();
                      });
                  },
                  load:function(){
                      for(var key in this.sprites){
                          this.sprites[key]=new Image();//объект для загрузки картинки
                          this.sprites[key].src='img/'+key+'.png';//путь к картинке
                      }
                      
                      
                  },
                  create:function(){
                      for(var row = 0; row < this.rows; row++){
                      for(var col = 0; col < this.cols; col++){
                          this.blocks.push({
                              x: 68 * col + 50,
                              y: 38 * row + 35,
                              width:64,
                              height:32,
                              isAlive: true
                              });
              
                          }
                      }
                  },
                  start: function(){
                      this.init();
                      this.load();
                      this.create();
                      this.run();
                  },
                  render: function(){
                      this.ctx.clearRect(0,0,this.width,this.height);
                      this.ctx.drawImage(this.sprites.background,0,0);//отрисовывает картинку, картинка и ее координаты х,у
                      this.ctx.drawImage(this.sprites.platform,this.platform.x,this.platform.y);
                      this.ctx.drawImage(this.sprites.ball,this.ball.x*this.ball.frame,0
                                        ,this.ball.width,this.ball.height
                                        ,this.ball.x,this.ball.y
                                        ,this.ball.width,this.ball.height);
                      
                      this.blocks.forEach(function(el){
                          if(el.isAlive){
                              this.ctx.drawImage(this.sprites.block,el.x,el.y);
                          }
                      },this)
              
                      
              
                      this.ctx.fillText('SCORE: '+this.score, 15,this.height-15);
                  },
                  update: function(){
                      if(this.ball.collide(this.platform)){
                          this.ball.bumpPlatform(this.platform);
                      }
              
              
                      if(this.platform.dx){
                          this.platform.move();
                      }
                      if(this.ball.dx||this.ball.dy){
                          this.ball.move();
                      }
                      this.blocks.forEach(function(element){
                          if(element.isAlive){
                                  if(this.ball.collide(element)){
                                      this.ball.bumpBlock(element);
                              }
                          }
                      },this);
                      this.ball.checkBounds();
                  },
                  run: function(){
                      this.update();
                      this.render();
                      if(this.running){
                          window.requestAnimationFrame(function(){
                              game.run();
                          });//указывает браузеру вывести на экран и запланировать перерисовку
                      }
                  },
                  over: function (mes) {
                      alert(mes);
                      console.log('GAME OVER!');
                      this.running=false;
                      window.location.reload();
                  }
              };
              
              game.ball={
                  width:22,
                  height:22,
                  frame:0,
                  x:340,
                  y:278,
                  dx:0,
                  dy:0,
                  speed:3,
                  jump:function(){
                      this.dx=-this.speed;
                      this.dy=-this.speed;
                      this.animate();
                  },
                  animate:function () {
                      setInterval(function() {
                          ++game.ball.frame;
                          if(game.ball.frame > 3){
                              game.ball.frame=0;
                          }
                      },100);
                      
                  },
                  move: function(){
                        this.x+=this.dx;
                        this.y+=this.dy;
                  },
                  collide:function(element){
                      var x=this.x + this.dx;
                      var y=this.y + this.dy;
                      if(x + this.width > element.x &&
                          x < element.x+element.width&&
                          y+this.height > element.y&&
                          y < element.y+element.height
                      ){
                          return true;
                      }
                      return false;
                  },
                  bumpBlock: function(block){
                      this.dy*=-1;
                      block.isAlive=false;
                      ++game.score;
                      if(game.score >= game.blocks.length){
                          game.over('YOU WIN');
                      }
                  },
                  onTheLeftSide:function (platform) {
                      return (this.x+this.width/2)<(platform.x+platform.width/2);
                  },
                  bumpPlatform: function(platform){
                      this.dy = -this.speed;
                      this.dx=this.onTheLeftSide(platform)?-this.speed:this.speed;
                      
                  },
                  checkBounds: function () {
                      var x=this.x + this.dx;
                      var y=this.y + this.dy;
                      if(x < 0){
                          this.x=0;
                          this.dx=this.speed;
                      }else if(x+this.width > game.width){
                          this.x=game.width-this.width;
                          this.dx=-this.speed;
                      }else if(y < 0){
                          this.y=0;
                          this.dy=this.speed;
                      }else if(y+this.height>game.height){
                          game.over('GAME OVER');
                      }
                  }
              }
              
              game.platform={
                  x:300,
                  y:300,
                  speed: 6,
                  dx: 0,
                  ball: game.ball,
                  width: 104,
                  height: 24,
                  move: function(){
                      this.x+=this.dx;
                      if(this.ball){
                          this.ball.x += this.dx;
                      }
                  },
                  stop: function(){
                      this.dx=0;
                      if(this.ball){
                          this.ball.dx = 0;
                      }
                  },
                  releaseBall:function(){
                      if(this.ball){
                          this.ball.jump();
                          this.ball=false;
                      }
                  } 
              };
              
              
              window.addEventListener('load',function(){
                  game.start();
              });

***

## PRACTIS:

1. Site: templa.by
2. Rsscool: Site LAMBDA
3. For test on platform: GeekBrains
4. For improve my skills on codewars: Codewars

## COURSE:

### HIGER ADUCATION
*2012* | Belarusian National Technical University

       | Instrument-making faculty, Micro and nanotechnics (engineer-technologist)

### PROFESSIONAL DEVELOPMENT, COURSES
*2018* | The basics of game development on Unity

       | HTP, Game Developer

*2014* | Software testing

       | Training Technology center "BelHard", SOFTWARE Testing (tester)

*2014* | Testing web-based applications

       | Training Technology center "BelHard", SOFTWARE Testing (tester)