---
toc: true
comments: false
layout: post
title: Sprtie animation 
description: sprite sheet assingment
type: tangibles 
courses: { compsci: {week: 4} }
---


<body>
    <div>
        <canvas id="spriteContainer"> <!-- Within the base div is a canvas. An HTML canvas is used only for graphics. It allows the user to access some basic functions related to the image created on the canvas (including animation) -->
            <img id="catSprite" src="{{site.baseurl}}/images/cat.png">  // change sprite here
        </canvas>
        <div id="controls"> <!--basic radio buttons which can be used to check whether each individual animaiton works -->
            <input type="radio" name="animation" id="walking" checked>
            <label for="walking">walking</label><br>
            <input type="radio" name="animation" id="turning">
            <label for="turning">turning</label><br>
            <input type="radio" name="animation" id="sitting">
            <label for="sitting">sitting</label><br>
        </div>
    </div>
</body>

<script>
    // start on page load
    window.addEventListener('load', function () {
        const canvas = document.getElementById('spriteContainer');
        const ctx = canvas.getContext('2d');
        const SPRITE_WIDTH = 130;  // matches sprite pixel width
        const SPRITE_HEIGHT = 130; // matches sprite pixel height
        const FRAME_LIMIT = 1;  // matches number of frames per sprite row, this code assume each row is same

        const SCALE_FACTOR = 2;  // control size of sprite on canvas
        canvas.width = SPRITE_WIDTH * SCALE_FACTOR;
        canvas.height = SPRITE_HEIGHT * SCALE_FACTOR;

        class Cat {
            constructor() {
                this.image = document.getElementById("catSprite");
                this.x = 0;
                this.y = 0;
                this.minFrame = 0;
                this.maxFrame = FRAME_LIMIT;
                this.frameX = 0;
                this.frameY = 0;
            }

            // draw cat object
            draw(context) {jukmlyik
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
                    this.frameX = 0;
                }
            }
        }

        // cat object
        const cat = new Cat();

        // update frameY of cat object, action from walking, sitting, turning radio control
        const controls = document.getElementById('controls');
        controls.addEventListener('click', function (event) {
            if (event.target.tagName === 'INPUT') {
                const selectedAnimation = event.target.id;
                switch (selectedAnimation) {
                    case 'walking':
                        cat.frameY = 0;
                        break;
                    case 'turning':
                        cat.frameY = 1;
                        break;
                    case 'sitting':
                        cat.frameY = 2;
                        break;
                    default:
                        break;
                }
            }
        });

        // Animation recursive control function
        function animate() {
            // Clears the canvas to remove the previous frame.
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draws the current frame of the sprite.
            cat.draw(ctx);

            // Updates the `frameX` property to prepare for the next frame in the sprite sheet.
            cat.update();

            // Uses `requestAnimationFrame` to synchronize the animation loop with the display's refresh rate,
            // ensuring smooth visuals.
            setTimeout(()=>{requestAnimationFrame(animate);},250);
        }

        // run 1st animate
        animate();
    });
</script>