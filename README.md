<!DOCTYPE html>
<!-- This is based on DillingerLee's great template here:
https://github.com/Team-Code/KA_Offline -->
<html> 
 <head>
    <title>Memory Game</title> 
</head>
 <body>
    <p align="center"> 
    <!--This draws the Canvas on the webpage -->
      <canvas id="mycanvas"></canvas> 
    </p>
 </body>
 
 <!-- Run all the JavaScript stuff -->
 <!-- Include the processing.js library -->
 <!-- See https://khanacademy.zendesk.com/hc/en-us/articles/202260404-What-parts-of-ProcessingJS-does-Khan-Academy-support- for differences -->
 <script src="https://cdn.jsdelivr.net/processing.js/1.4.8/processing.min.js"></script>

 <script>
    var MemoryGame = function(processingInstance) {
     with (processingInstance) {
        var canvasSize = 600;
        size(canvasSize,canvasSize); 
        frameRate(30);
        
        // ProgramCodeGoesHere

var waitHere = 0;
var noHover = color(214, 247, 202);
var yesHover = color(173, 60, 186);
var blk = color(0, 0, 0);
var clkclr = color(240, 166, 158);
var startTime = 0;
var endTime = 0;
var k = 0;
//var photoLocation = "c:/Users/Ken/Documents/Programs/memorygame/Images/"
var photoLocation = ""

// Array for holding game tiles
var tiles = [];

// Global config
var NUM_COLS = 4;
var NUM_ROWS = 4;

// array to be used for randomizing tile images
var selected = [];

// Define the Tile function for filling tile array
var Tile = function(x, y, width, face) {
    this.x = x;
    this.y = y;
    this.width = width;
    this.face = face;
    this.isFaceUp = false;
    this.isMatch = false;
    this.bkGrnd = noHover;
};

var sizeFactor = 0.60;
var shiftFactor = (1.0 - sizeFactor)/2;
// Define the tile drawing function
Tile.prototype.draw = function() {
    fill(this.bkGrnd);
    strokeWeight(2);
    rect(this.x, this.y, this.width, this.width, 10);
    if (this.isFaceUp) {
        image(this.face, this.x, this.y, this.width, this.width);
    } else {

        image(tileBack, this.x+this.width*shiftFactor, this.y+this.width*shiftFactor, this.width*sizeFactor, this.width*sizeFactor);
    }
};

// Define function to check to see if mouse is over tile
Tile.prototype.isUnderMouse = function(x, y) {
    return x >= this.x && x <= this.x + this.width  &&
        y >= this.y && y <= this.y + this.width;
};

// Declare an array of all possible faces (Min of 32)


i0 = loadImage(photoLocation+"p0.jpg");
i1 = loadImage(photoLocation+"p1.jpg");
i2 = loadImage(photoLocation+"p2.jpg");
i3 = loadImage(photoLocation+"p4.jpg");
i4 = loadImage(photoLocation+"p5.jpg");
i5 = loadImage(photoLocation+"p5.jpg");
i6 = loadImage(photoLocation+"p6.jpg");
i7 = loadImage(photoLocation+"p7.jpg");
i8 = loadImage(photoLocation+"p8.jpg");
i9 = loadImage(photoLocation+"p9.jpg");
i10 = loadImage(photoLocation+"p10.jpg");
i11 = loadImage(photoLocation+"p11.jpg");
i12 = loadImage(photoLocation+"p12.jpg");
i13 = loadImage(photoLocation+"p13.jpg");
i14 = loadImage(photoLocation+"p14.jpg");
i15 = loadImage(photoLocation+"p15.jpg");
i16 = loadImage(photoLocation+"p16.jpg");
i17 = loadImage(photoLocation+"p17.jpg");
i18 = loadImage(photoLocation+"p18.jpg");
i19 = loadImage(photoLocation+"p19.jpg");
i20 = loadImage(photoLocation+"p20.jpg");
i21 = loadImage(photoLocation+"p21.jpg");
i22 = loadImage(photoLocation+"p22.jpg");
i23 = loadImage(photoLocation+"p23.jpg");
i24 = loadImage(photoLocation+"p24.jpg");
i25 = loadImage(photoLocation+"p25.jpg");
i26 = loadImage(photoLocation+"p26.jpg");
i27 = loadImage(photoLocation+"p27.jpg");
i28 = loadImage(photoLocation+"p28.jpg");
i29 = loadImage(photoLocation+"p29.jpg");
i30 = loadImage(photoLocation+"p30.jpg");
i31 = loadImage(photoLocation+"p31.jpg");
i32 = loadImage(photoLocation+"p32.jpg");
i33 = loadImage(photoLocation+"p33.jpg");
i34 = loadImage(photoLocation+"p34.jpg");
i35 = loadImage(photoLocation+"p35.jpg");
i36 = loadImage(photoLocation+"p36.jpg");
i37 = loadImage(photoLocation+"p37.jpg");
i38 = loadImage(photoLocation+"p38.jpg");
i39 = loadImage(photoLocation+"p39.jpg");


var faces = [
    i0,i1,i2,i3,i4,i5,i6,i7,i8,i9,i10,i11,i12,i13,i14,i15,i16,i17,i18,i19,i20,i21,i22,i23,i24,i25,i26,i27,i28,i29,i30,i31,i32,i33,i34,i35,i36,i37,i38,i39
];

tileBack = faces[floor(random(faces.length))]

// Make an temporary array which has 2 of each, then randomize it for selection purposes
var makeArray = function(selected) {
var possibleFaces = faces.slice(0);
i=floor(random(possibleFaces.length));
tileBack = possibleFaces[(i)];
possibleFaces.splice(i, 1);

    for (var i = 0; i < (NUM_COLS * NUM_ROWS) / 2; i++) {
        // Randomly pick one from the array of remaining faces
        var randomInd = floor(random(possibleFaces.length));
        var face = possibleFaces[randomInd];
        // Push twice onto array
        selected.push(face);
        selected.push(face);
        // Remove from array
        possibleFaces.splice(randomInd, 1);

    }
};


// Function to shuffle the elements of array
var shuffleArray = function(array) {
    var counter = array.length;

    // While there are elements in the array
    while (counter > 0) {
        // Pick a random index
        var ind = Math.floor(Math.random() * counter);
        // Decrease counter by 1
        counter--;
        // And swap the last element with it
        var temp = array[counter];
        array[counter] = array[ind];
        array[ind] = temp;
    }
};


// Create the tiles
var createTiles = function(tiles) {
    var sz = 0;
    var gap = 4;
    var ygap = 0;
    var xgap = 0;
    sz = max(NUM_COLS , NUM_ROWS);
    var i=floor(((width-10)-(gap*(NUM_COLS)))/NUM_COLS);
    var j=floor(((width-40)-(gap*(NUM_ROWS)))/NUM_ROWS);
    sz = min(i,j);
    xgap = (width - (NUM_COLS * (sz+gap)))/2;
    ygap = ((width-35) - (NUM_ROWS * (sz+gap)))/2;
    for (var i = 0; i < NUM_COLS; i++) {
        for (var j = 0; j < NUM_ROWS; j++) {
            var tileX = i * (sz+gap) + xgap;
            var tileY = j * (sz+gap) + ygap;
            var tileFace = selected.pop();
            var tileWidth = sz;
            tiles.push(new Tile(tileX, tileY, tileWidth, tileFace));
        }
}

background(255, 255, 255);


};

var numTries = 0;
var numMatches = 0;
var flippedTiles = [];
var delayStartFC = null;


// Function to handle mouse clicked scenarios
mouseClicked = function() {

// Set Game Parameters and Begin Scenario
    if(waitHere === 0) {
        // Check for Row and Column Selection Click   

        for (var i=2; i<9; i++) {
            if(mouseX>=20+40*(i-2) && mouseX<=20+40*(i-2)+40 && mouseY>=60 && mouseY<=100) {
                NUM_COLS=i;
                draw()

            }
            if(mouseX>=20+40*(i-2) && mouseX<=20+40*(i-2)+40 && mouseY>=160 && mouseY<=200) {
                NUM_ROWS=i;
                draw()

            }

        }

        // Check for "Continue" Click if Even Number of Tiles 
        if(mouseX>=100 && mouseX<=200 && mouseY>=height-40 && mouseY<=height-10  && (NUM_COLS*NUM_ROWS)/2-round((NUM_COLS*NUM_ROWS)/2) === 0) {
            waitHere = 1;
            for (i=1; i<10000; i++) {
            
            }
            makeArray(selected);
            shuffleArray(selected);
            createTiles(tiles);
            numTries = 0;
            numMatches = 0;
            delayStartFC = null;
            background(88, 206, 227);
            // Start Game Timer
            startTime = millis();
            draw()
//            return;
        }
        draw()
//        return;
    }
    
    //Check for "Flip all Tiles" Button
    if(mouseX>=400 && mouseX<=520 && mouseY>=height-35 &&mouseY<=height-5 && waitHere > 0) {
        for (var i = 0; i < tiles.length; i++) {
            var tile = tiles[i];
            
            if (!tile.isFaceUp) {
                tile.isFaceUp = true;
                flippedTiles.push(tile);
            } 
            loop();
            
        }        
    }
        
    // Restart after Finishing Game or "Restart" during game

    if(mouseX>=290 && mouseX<=380 && mouseY>=height-35 &&mouseY<=height-5 && waitHere > 0) {
        waitHere = 0;
        tiles.length = 0;
        selected.length = 0;
        tileBack = faces[floor(random(faces.length))]    
       draw();
    }
    
    // Check Tiles
    for (var i = 0; i < tiles.length; i++) {
        var tile = tiles[i];
        if (tile.isUnderMouse(mouseX, mouseY)) {
            if (flippedTiles.length < 2 && !tile.isFaceUp) {
                tile.isFaceUp = true;
                flippedTiles.push(tile);
                if (flippedTiles.length === 2) {
                    numTries++;
                    if (flippedTiles[0].face === flippedTiles[1].face) {
                        flippedTiles[0].isMatch = true;
                        flippedTiles[1].isMatch = true;
                        flippedTiles.length = 0;
                        numMatches++;
                    }
                    delayStartFC = frameCount;
                }
            } 
            loop();
        }
    }
};

//Starting page for selecting grid size
var startPage = function() {
    background(88, 206, 227);

    textSize(20);
    fill(blk);
    textAlign(CENTER);
    text("Number of Columns (2-8) : ",150,40);
    text("Number of Rows (2-8)      : ",150,140);

    // Check for Even Number of Tiles
    if((NUM_COLS*NUM_ROWS)/2-round((NUM_COLS*NUM_ROWS)/2) === 0) {
        text("Click Continue Button to Start",150,height-50);
        fill(clkclr);
        rect(100,height-40,100,30);
        fill(blk);
        text("Continue", 150,height-17);

    } else {
        fill(255, 0, 0);
        text("Number of Rows x Number of Columns\nMust be an Even Number!\n(Not 3x3,3x5,3x7,5x5,5x7, or 7x7)",200,250);
        fill(blk);
    }    
    stroke(blk);
    strokeWeight(2);
    for (var i=2; i<9; i++) {
        fill(noHover);
        if (i===NUM_COLS) {
            fill(clkclr);
        }
        rect(20+40*(i-2),60,40,40);
        fill(blk);
        text(i,40+(i-2)*40,88);

        fill(noHover);
        if (i===NUM_ROWS) {
            fill(clkclr);
        }
        rect(20+40*(i-2),160,40,40);
        fill(blk);
        text(i,40+(i-2)*40,188);
    }
// display all photos

        for (var i=0; i<faces.length; i++) {
            if (i<10) {
                j=i;
                k=360;
            }
            if (i>=10 && i<20) {
                j=i-10;
                k=420;
            }
            if (i>=20 && i<30) {
                j=i-20;
                k=480;
            }
            if (i>=30 && i<40) {
                j=i-30;
                k=540;
            }
            image(faces[i], k,j*60,58,58);
        }


};

draw = function() {
    // If just starting, go to startPage function
    if (waitHere === 0) {
        startPage();
    // otherwise play game
    } else {
        background(255, 255, 255);
        if (delayStartFC && (frameCount - delayStartFC) > 30) {
            for (var i = 0; i < tiles.length; i++) {
                var tile = tiles[i];
                if (!tile.isMatch) {
                    tile.isFaceUp = false;
                }
            }
            flippedTiles = [];
            delayStartFC = null;
        }
    
    // Change tile color when mouse is overhead
        for (var i = 0; i < tiles.length; i++) {
            if (tiles[i].isUnderMouse(mouseX,mouseY)) {
                tiles[i].bkGrnd = yesHover;
            } else {
                tiles[i].bkGrnd = noHover;
            }
            if (tiles[i].isFaceUp && !tiles[i].isMatch) {
                tiles[i].bkGrnd = yesHover;
            }
            tiles[i].draw();
            fill(184, 44, 58);
            rect(290,height-35,90,30);
            fill(0,0,0);
            textSize(20);
            text("Restart",335,height-15);

            fill(184, 44, 58);
            rect(400,height-35,120,30);
            fill(0,0,0);
            textSize(20);
            text("Flip all Tiles",460,height-15);

        }
    
    // Has game been completed?
        if (numMatches === tiles.length/2) {
            fill(0, 0, 0);
            textSize(15);
            // Calculate Game Time
            if(endTime === 0) {
                endTime = round((millis() - startTime)/1000);
            }
            text("Completed! : " + numTries + " moves / "+ endTime + " seconds", 140, height-15);
            fill(184, 44, 58);
            rect(290,height-35,90,30);
            fill(0,0,0);
            textSize(20);
            text("Continue",335,height-15);
            waitHere = 2;
            noLoop();
        }
    }
};

//noLoop();


    }};
    // Get the canvas that Processing-js will use
    var canvas = document.getElementById("mycanvas"); 
    // Pass the function sketchProc (defined in myCode.js) to Processing's constructor.
    var processingInstance = new Processing(canvas, MemoryGame); 
 </script>

</html>
