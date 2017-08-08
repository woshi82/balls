/*
 * @Author: yourName
 * @Date: 2017-04-28 16:20:38
 * @Last Modified by: zengyanling
 * @Last Modified time: 2017-05-04 09:50:04
 */
<template src="./index.html"></template>
<script type="text/javascript">
// import * as JC from 'jcc2d/src/index';
// import * as JC from 'jcc2d';
import Matter from 'libs/matter-dev';
// TODO: 动态加载
import g1 from '../../../assets/images/g1.png';
import g2 from '../../../assets/images/g2.png';
import g3 from '../../../assets/images/g3.png';
import g4 from '../../../assets/images/g4.png';
import g5 from '../../../assets/images/g5.png';
import g6 from '../../../assets/images/g6.png';
import g7 from '../../../assets/images/g7.png';
import g8 from '../../../assets/images/g8.png';
import g9 from '../../../assets/images/g9.png';
import g10 from '../../../assets/images/g10.png';
import b1 from '../../../assets/images/b1.png';
import b2 from '../../../assets/images/b2.png';
import b3 from '../../../assets/images/b3.png';
import b4 from '../../../assets/images/b4.png';
import b5 from '../../../assets/images/b5.png';
import b6 from '../../../assets/images/b6.png';
import b7 from '../../../assets/images/b7.png';
import b8 from '../../../assets/images/b8.png';
import b9 from '../../../assets/images/b9.png';
import b10 from '../../../assets/images/b10.png';

export default {
    data() {
        return {
            sOritation: '',
            infoH: '11222ddd',
            infoV: '11222ddd'
        };
    },
    mounted() {

        // this.ball1();
        // this.balls();
        // this.matterExample();
        this.spriteExample();
        // this.motionExample();
    },
    methods: {
        motionExample(gravity){
            const self = this;
            let alpha;
            let beta;
            let gamma;

            window.addEventListener('deviceorientation', function(event) {
                alpha = event.alpha;
                beta = event.beta;
                gamma = event.gamma;
                // // console.log(event)
                // // 处理event.alpha、event.beta及event.gamma
                if (alpha != null || beta != null || gamma != null) {
                //     self.sOritation = 'alpha:' + parseInt(alpha) + '<br />beta:' + parseInt(beta) + '<br />gamma:' + parseInt(gamma);
                //     //判断屏幕方向
                //     if (gamma > 0) {
                //         self.infoH = '向右倾斜';
                //     } else {
                //         self.infoH = '向左倾斜';
                //     }
                //     if (beta > 0) {
                //         self.infoV = '向下倾斜';
                //     } else {
                //         self.infoV = '向上倾斜';
                //     }
                //     // 0~20
                //     // gravity.x = Math.PI * gamma / 180;
                //     // gravity.y = Math.PI * bata / 180;
                //     gravity.x = parseFloat(gamma/9);
                //     gravity.y = parseFloat(beta/18);

                    var orientation = typeof window.orientation !== 'undefined' ? window.orientation : 0;

                    if (orientation === 0) {
                        gravity.x = Matter.Common.clamp(event.gamma, -90, 90) / 90*4;
                        gravity.y = Matter.Common.clamp(event.beta, -90, 90) / 90*4;
                    } else if (orientation === 180) {
                        gravity.x = Matter.Common.clamp(event.gamma, -90, 90) / 90*4;
                        gravity.y = Matter.Common.clamp(-event.beta, -90, 90) / 90*4;
                    } else if (orientation === 90) {
                        gravity.x = Matter.Common.clamp(event.beta, -90, 90) / 90*4;
                        gravity.y = Matter.Common.clamp(-event.gamma, -90, 90) / 90*4;
                    } else if (orientation === -90) {
                        gravity.x = Matter.Common.clamp(-event.beta, -90, 90) / 90*4;
                        gravity.y = Matter.Common.clamp(event.gamma, -90, 90) / 90*4;
                    }
                    self.infoH = gravity.x;
                } else {
                    self.sOritation = '当前浏览器不支持DeviceOrientation';
                }
            }, true);
        },
        spriteExample(){
            const aGImgs = [g1, g2, g3, g4, g5, g6, g7, g8, g9, g10, ];
            const aBImgs = [b1, b2, b3, b4, b5, b6, b7, b8, b9, b10 ];
            const W = window.innerWidth;
            const H = window.innerHeight;
            const pixelRatio = window.devicePixelRatio;
            const maxH = 250;
            // module aliases
            var Engine = Matter.Engine,
                Render = Matter.Render,
                World = Matter.World,
                Composites = Matter.Composites,
                Mouse = Matter.Mouse,
                MouseConstraint = Matter.MouseConstraint,
                Bodies = Matter.Bodies;

            // create an engine
            var engine = Engine.create();
            var engineWorld = engine.world;


            // create a renderer
            var render = Render.create({
                element: document.querySelector('.index'),
                engine: engine,
                options: {
                    wireframes: false,
                    // showAngleIndicator: true,
                    // showDebug: true,
                    width: W,
                    height: maxH,
                    pixelRatio: 'auto',
                    background: '#363f5d'
                }
            });


            // add bodies
            const boundWidth = 10;
            const boundOpt = {
                isStatic: true,
                render: {
                    fillStyle: '#03A9F4',
                }
            };
            // console.log(Matter.Common.clamp(20, -90, 90) / 90)
            // const maxH = 300;

            World.add(engineWorld, [
                // 以物体的圆心为原点
                // 上下左右
                Bodies.rectangle(W/2, -boundWidth/2, W, boundWidth, boundOpt),
                Bodies.rectangle(W/2, maxH + boundWidth/2 , W, boundWidth, boundOpt),
                Bodies.rectangle(-boundWidth/2, maxH/2, boundWidth, maxH, boundOpt),
                Bodies.rectangle(W + boundWidth/2, maxH/2, boundWidth, maxH, boundOpt),
            ]);
            // b.bounds.min.x = 0;
            // b.bounds.min.y = 0;
            engineWorld.bounds.max.x = W;
            engineWorld.bounds.max.y = maxH;
            let iG = 0;
            let iB = 0;
            let texture = null;
            let num = 14;
            let iRow = 0;
            let iCol = 0;
            let r = 20;
            const aCircle = [];
            let x = 0;
            let y = 0;
            for(var i = 0; i < num; i++){
                if(Math.round(Math.random())){
                    iG = iG >= 9 ? 0 : ++iG;
                    texture = aGImgs[iG];
                } else {
                    iB = iB >= 9 ? 0 : ++iB;
                    texture = aBImgs[iB];

                }
                x = x + 2 * r + (Math.random() * 5 + 5);
                if(x > W - r ){
                    iCol = i ;
                    iRow++;
                    x = r + (Math.random() * 5 + 5);
                }
                y = r + iRow * 2 * r;
                console.log(y)
                aCircle.push(
                    Bodies.circle(x, y, r, {
                        // density: 0.0005, // 密度
                        // frictionAir: 0.06, // 空气摩擦力
                        restitution: 0.4, // 恢复
                        friction: 0.01, // 摩擦力
                        // chamfer: {
                        //     radius: 20
                        // },
                        render: {
                            // fillStyle: '#000',
                            sprite: {
                                texture: texture,
                                xScale: 0.308, // 缩放位图
                                yScale: 0.308,
                            }
                        }
                    })
                );
            }
            World.add(engineWorld, aCircle);

            var stack = Composites.stack(10, 10, 8, 1.5, 10, 10, function(x, y) {
                if(Math.round(Math.random())){
                    iG = iG >= 9 ? 0 : ++iG;
                    texture = aGImgs[iG];
                } else {
                    iB = iB >= 9 ? 0 : ++iB;
                    texture = aBImgs[iB];

                }
                // console.log(i, aImgs[i])
                const b =  Bodies.circle(x, y, 20, {
                    // density: 0.0005, // 密度
                    // frictionAir: 0.06, // 空气摩擦力
                    restitution: 0.4, // 恢复
                    friction: 0.01, // 摩擦力
                    // chamfer: {
                    //     radius: 20
                    // },
                    render: {
                        // fillStyle: '#000',
                        sprite: {
                            texture: texture,
                            xScale: 0.308, // 缩放位图
                            yScale: 0.308,
                        }
                    }
                });

                return b;
            });
            // World.add(engineWorld, stack);


            this.motionExample(engineWorld.gravity);

            // add mouse control
            var mouse = Mouse.create(render.canvas);
            var mouseConstraint = MouseConstraint.create(engine, {
                    mouse: mouse,
                    constraint: {
                        stiffness: 0.2,
                        render: {
                            visible: false
                        }
                    }
                });
            // fit the render viewport to the scene
            // Render.lookAt(render, {
            //     min: { x: 0, y: 0 },
            //     max: { x: W, y: maxH }
            // });
            World.add(engineWorld, mouseConstraint);

            // keep the mouse in sync with rendering
            render.mouse = mouse;

            // run the engine
            Engine.run(engine);

            // run the renderer
            Render.run(render);
        },
        matterExample(){
            const W = window.innerWidth;
            const H = window.innerHeight;
            const pixelRatio = window.devicePixelRatio;

            // module aliases
            var Engine = Matter.Engine,
                Render = Matter.Render,
                World = Matter.World,
                Bodies = Matter.Bodies;

            // create an engine
            var engine = Engine.create();

            // create a renderer
            var render = Render.create({
                element: document.querySelector('.index'),
                engine: engine,
                options: {
                    wireframes: false,
                    width: W,
                    height: H,
                    pixelRatio: pixelRatio,
                    background: '#363f5d'
                }
            });

            // create two boxes and a ground
            var boxA = Bodies.rectangle(40, 200, 80, 80, {
                sprite: {
                    texture: ball
                }
            });
            var boxB = Bodies.rectangle(40, 50, 80, 80, {
                background: '#4d90fe'
            });
            var ground = Bodies.rectangle(400, 610, 810, 60, { isStatic: true });

            // add all of the bodies to the world
            World.add(engine.world, [boxA, boxB, ground]);

            // run the engine
            Engine.run(engine);

            // run the renderer
            Render.run(render);
        },
        balls(){
            const stage = new JC.Stage({
                dom: 'balls',
                bgColor: 'grey',
                width: window.innerWidth,
                height: window.innerHeight,
            });
            // 加载资源
            var loadBox = JC.loaderUtil({
                ball: ball
            });

            const num = 2;
            let ballC = [];
            const R = 30;
            const W = window.innerWidth;
            const H = window.innerHeight;
            const bound = { // 边界
                y: {
                    min: 0,
                    max: 200
                },
                x: {
                    min: 0,
                    max: W - 2*R
                },
            }
            let acceA = 1; // 加速度
            // let angleA = 0
            let angleA = Math.PI/6;

            let acce = {
                x: 10,
                y: acceA,
            };
            // let angleA = Math.PI/6;
            let acce1 = {
                x: acceA * Math.sin(angleA),
                y: acceA * Math.cos(angleA),
            };
            let acce2 = {
                x: acceA * Math.sin(Math.PI/6),
                y: acceA * Math.cos(Math.PI/6),
            };

            // 创建球
            for(var i = 0, len = num; i < len; i++){
                (function(i){
                    ballC.push({
                        sprite: new JC.Sprite({
                            texture: loadBox.getById('ball'),
                            // x: 80,
                            // y: 200,
                            width: 60,
                            height: 60,
                            animations: {
                                fall: {start: 0,end: 4,next: 'stand'},
                                fly: {start: 5,end: 9,next: {movie: 'stand', repeats: 2}},
                                stand: {start: 10,end: 39},
                                walk: {start: 40,end: 59,next: 'stand'},
                                other: [ 0, 1, 2, 1, 3, 4 ], // 同样接受数组形势
                            }
                        }),
                        v: {
                            x: 0,
                            y: 0,
                        },
                        btn: {
                            x: true,
                            y: true,
                        },
                        acce: {
                            x: acceA * Math.sin(angleA),
                            y: acceA * Math.cos(angleA),
                        },
                        timer: null,
                    });

                    ballC[i].pivotX = R;
                    ballC[i].pivotY = R;
                    ballC[i].sprite.x = 69 * i;
                    move(i);
                    stage.adds(ballC[i].sprite);
                })(i);
            }


            stage.startEngine();

            function detectBound(){
                let a = {};
                let b = {};
                let F1 = 0; // 相互作用力
                let Fa = 0; // 相互作用力
                let Fb = 0; // 相互作用力
                let angleF1 = 0; // 相互作用力夹角
                let angleFa = 0; // 相互作用力夹角
                let angleFb = 0; // 相互作用力夹角
                let angleVa = 0; // a速度的角度
                let angleVb = 0; // b速度的角度
                let temV = { x: 0, y: 0};

                let unitVectorAB = { x: 0, y: 0};
                let moAB = 0;
                let deltR = 0;
                for(var i = 0, len = num-1; i < len; i++){
                    a = {
                        x: ballC[i].sprite.x,
                        y: ballC[i].sprite.y,
                    };
                    b = {
                        x: ballC[i+1].sprite.x,
                        y: ballC[i+1].sprite.y,
                    };
                    moAB = getDis(a, b);
                    if(moAB <= 2*R){
                        // TODO: 挪位
                        unitVectorAB = {
                            x: (b.x - a.x)/ moAB,
                            y: (b.y - a.y)/ moAB,
                        }
                        deltR = (2*R - moAB);
                        // 挪a的位置
                        a = {
                            x: a.x + unitVectorAB.x * deltR,
                            y: a.y + unitVectorAB.y * deltR,
                        }
                        ballC[i].sprite.x = a.x;
                        ballC[i].sprite.y = a.y;

                        // ballC[i].v = {
                        //     x: (a.x - b.x) * ballC[i].v.x * unitVectorAB.x,
                        //     y: (a.y - b.y) * ballC[i].v.y * unitVectorAB.y,
                        // }
                        // ballC[i+1].v = {
                        //     x: - (a.x - b.x) * ballC[i+1].v.x * unitVectorAB.x,
                        //     y: - (a.y - b.y) * ballC[i+1].v.y * unitVectorAB.y,
                        // }

                        // b = {
                        //     x: b.x - unitVectorAB.x * deltR,
                        //     y: b.y - unitVectorAB.y * deltR,
                        // }
                        // ballC[i+1].sprite.x = b.x;
                        // ballC[i+1].sprite.y = b.y;

                        // clearInterval(ballC[i].timer)
                        // temV = {
                        //     x: ballC[i].v.x,
                        //     y: ballC[i].v.y,
                        // }
                        // ballC[i].v = {
                        //     x: ballC[i+1].v.x,
                        //     y: ballC[i+1].v.y,
                        // }
                        // ballC[i+1].v = {
                        //     x: temV.x,
                        //     y: temV.y,
                        // }
                    }

                }
            }
            function getDis(a, b){
                // TODO: 优化
                return Math.sqrt(Math.pow(b.x-a.x, 2) + Math.pow(b.y-a.y, 2))
                // return Math.pow(b.x-a.x, 2) + Math.pow(b.y-a.y, 2)
            }

            function move(i) {
                ballC[i].timer = setInterval(function() {
                    if(ballC[i].sprite.y >= bound.y.max){
                        ballC[i].v.y = -ballC[i].v.y;
                        ballC[i].sprite.y = bound.y.max;
                    } else if(ballC[i].sprite.y <= bound.y.min) {
                        ballC[i].v.y = -ballC[i].v.y;
                        ballC[i].sprite.y = bound.y.min;
                    }
                    if(ballC[i].sprite.x >= bound.x.max){
                        ballC[i].v.x = -ballC[i].v.x;
                        ballC[i].sprite.x = bound.x.max;
                    } else if(ballC[i].sprite.x <= bound.x.min) {
                        ballC[i].v.x = -ballC[i].v.x;
                        ballC[i].sprite.x = bound.x.min;
                    }
                    if(Math.abs(ballC[i].v.y) <= 0.05 && ballC[i].sprite.y >= bound.y.max) {
                        ballC[i].v.y = 0;
                        ballC[i].btn.y = false;
                    };
                    //
                    if(Math.abs(ballC[i].v.x) <= 0.05 && ballC[i].sprite.x >= bound.x.max ) {
                        ballC[i].v.x = 0;
                        ballC[i].btn.x = false;
                    };
                    // console.log(ballC[i].sprite.x, ballC[i].sprite.y)
                    // !ballC[i].btn.x && !ballC[i].btn.y && clearInterval(timer);

                        if(ballC[i].btn.y){
                            ballC[i].v.y += ballC[i].acce.y;
                            ballC[i].sprite.y += ballC[i].v.y;
                        }
                        if(ballC[i].btn.x){
                            ballC[i].v.x += ballC[i].acce.x;
                            ballC[i].sprite.x += ballC[i].v.x;
                        }
                    detectBound();

                }, 1000/30);
            };


        },
        ball1() {
            const stage = new JC.Stage({
                dom: 'balls',
                bgColor: 'yellow',
                width: window.innerWidth,
                height: window.innerHeight,
                // resolution: Math.min(1, window.devicePixelRatio),
                // rem: 75,
            });
            var loadBox = JC.loaderUtil({
                ball: ball
            });
            var sprite = new JC.Sprite({
                texture: loadBox.getById('ball'),
                // frame: new JC.Rectangle(0, 0,window.innerWidth, window.innerHeight),
                // frame: new JC.Rectangle(0, 0, 36, 36),
                x: 0,
                y: 0,
                width: 60,
                height: 60,
                animations: {
                    fall: {start: 0,end: 4,next: 'stand'},
                    fly: {start: 5,end: 9,next: {movie: 'stand', repeats: 2}},
                    stand: {start: 10,end: 39},
                    walk: {start: 40,end: 59,next: 'stand'},
                    other: [ 0, 1, 2, 1, 3, 4 ], // 同样接受数组形势
                }
            });
            const W = window.innerWidth;
            const H = window.innerHeight;
            const bound = { // 边界
                y: {
                    min: 0,
                    max: 2
                },
                x: {
                    min: 0,
                    max: W - 60
                },
            }
            let a = 10; // 加速度
            let v = {
                x: 0,
                y: 0,
            };
            let deg = 30; //
            let acce = {
                x: a * Math.sin(Math.PI/6),
                y: a * Math.cos(Math.PI/6),
            };
            let btn = {
                x: true,
                y: true,
            }
            const timer = setInterval(function() {
                if(sprite.y >= bound.y.max){
                    v.y = -v.y;
                    sprite.y = bound.y.max;
                }
                if(sprite.x >= bound.x.max){
                    v.x = -v.x;
                    sprite.x = bound.x.max;
                } else if(sprite.x <= bound.x.min) {
                    v.x = -v.x;
                    sprite.x = bound.x.min;
                }
                if(Math.abs(v.y) <= 0.05 && sprite.y >= bound.y.max) {
                    v.y = 0;
                    btn.y = false;
                };
                if(Math.abs(v.x) <= 0.05 && sprite.x >= bound.x.max ) {
                    v.x = 0;
                    btn.x = false;
                };
                console.log(sprite.x, sprite.y)
                !btn.x && !btn.y && clearInterval(timer);
                // 假设此时的受力
                // if(v.y > 100){
                    if(btn.y){
                        v.y += acce.y;
                        sprite.y += v.y;
                    }
                    if(btn.x){
                        v.x += acce.x;
                        sprite.x += v.x;
                    }

                // } else {
                //     v.y += a;
                //     sprite.y += v.y;

                // }


            }, 1000/30);

            stage.adds(sprite);
            stage.startEngine();
        },
    },
};
</script>
 <style lang="scss" src="./index.scss"></style>

