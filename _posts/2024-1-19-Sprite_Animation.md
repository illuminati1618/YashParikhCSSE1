---
layout: post
title: Sprite animation 1
description: A hand-drawn sprite animation of glass
type: hacks
courses: {'compsci': {'week': 7}}
---


<body>
    <div>
        <canvas id="spriteContainer"> <!-- Within the base div is a canvas. An HTML canvas is used only for graphics. It allows the user to access some basic functions related to the image created on the canvas (including animation) -->
            <img id="dogSprite" src="{{site.baseurl}}/images/sprite/combinedglassspritesheet.png">  // change sprite here
        </canvas>
        <div id="controls"> <!--basic radio buttons which can be used to check whether each individual animaiton works -->
            <input type="radio" name="animation" id="idle" checked>
            <label for="idle">Shimmering</label><br>
            <input type="radio" name="animation" id="barking">
            <label for="barking">Cracking</label><br>
            <input type="radio" name="animation" id="walking">
            <label for="walking">Breaking</label><br>
        </div>
    </div>
</body>

<script>
    // start on page load
    window.addEventListener('load', function () {
        const canvas = document.getElementById('spriteContainer');
        const ctx = canvas.getContext('2d');
        const SPRITE_WIDTH = 32;  // matches sprite pixel width
        const SPRITE_HEIGHT = 32; // matches sprite pixel height
        const FRAME_LIMIT = 27;  // matches number of frames per sprite row, this code assume each row is same

        const SCALE_FACTOR = 5;  // control size of sprite on canvas
        canvas.width = SPRITE_WIDTH * SCALE_FACTOR;
        canvas.height = SPRITE_HEIGHT * SCALE_FACTOR;

        class Dog {
            constructor() {
                this.image = document.getElementById("dogSprite");
                this.x = 0;
                this.y = 0;
                this.minFrame = 0;
                this.maxFrame = FRAME_LIMIT;
                this.frameX = 0;
                this.frameY = 0;
            }

            // draw dog object
            draw(context) {
                context.drawImage(
                    this.image,
                    this.frameX * SPRITE_WIDTH,
                    this.frameY * SPRITE_HEIGHT,
                    SPRITE_WIDTH,
                    SPRITE_HEIGHT,
                    this.x,
                    this.y,
                    canvas.width,
                    canvas.height
                );
            }

            // update frameX of object
            update() {
                if (this.frameX < this.maxFrame) {
                    this.frameX++;
                } else {
                    this.frameX = this.minFrame;
                }
            }
        }

        // dog object
        const dog = new Dog();

        // update frameY of dog object, action from idle, bark, walk radio control
        const controls = document.getElementById('controls');
        controls.addEventListener('click', function (event) {
            if (event.target.tagName === 'INPUT') {
                const selectedAnimation = event.target.id;
                switch (selectedAnimation) {
                    case 'idle':
                        dog.frameY = 0;
                        break;
                    case 'barking':
                        dog.frameY = 2;
                        break;
                    case 'walking':
                        dog.frameY = 1;
                        break;
                    default:
                        break;
                }
            }
        });

        // Animation recursive control function
        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            dog.draw(ctx);
            dog.update();
            setTimeout(function() {
                dog.update();
                requestAnimationFrame(animate);
            }, 150);
        }

        // run 1st animate
        animate();
    });
</script>