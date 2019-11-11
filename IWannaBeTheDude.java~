/* I Wanna Be The Chap
 * Platformer with shooting mechanics, taken inspirtation from I Wanna Be The Guy 
 * @author Arman
 * @version June 2018
 */
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.io.*;
import java.util.*; 
import javax.imageio.ImageIO;
import java.awt.image.BufferedImage;
import java.io.File;
import javax.sound.sampled.*;

public class IWannaBeTheDude{
    //Game window properties 
    static JFrame gameWindow; 
    static GraphicsPanel canvas; 
    static final int WIDTH = 815;
    static final int HEIGHT = 600; 
    static final int TOP = 0; 
    static final int LEFT = 0;
    //Key listener 
    static MyKeyListener keyListener = new MyKeyListener(); 
    //Background properties
    static BufferedImage mainMenu;
    static BufferedImage controlScreen; 
    static BufferedImage levelTwoBackground; 
    static BufferedImage gameOverScreen; 
    static BufferedImage levelOneBackground; 
    static BufferedImage levelCompleteScreen; 
    static BufferedImage saveSelectScreen; 
    static BufferedImage endScreen; 
    static final int LEVEL_X = 0; 
    static final int LEVEL_Y = 0; 
    //Character properties 
    static boolean firstSpawn = true; 
    static int characterX;
    static int characterY;
    static int lvOneCharacterX = 45;
    static int lvOneCharacterY = 513;
    static int lvTwoCharacterX = 50; 
    static int lvTwoCharacterY = 285; 
    static int characterW = 15;
    static int characterH = 25; 
    static String characterState = "standing right";
    static int characterPicNum; 
    static BufferedImage[] characterSprites = new BufferedImage[12];
    static int[] nextLeftPic  = {0, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0};
    static int[] nextRightPic = {3, 3, 3, 3, 4, 5, 4, 3, 3, 3, 3, 3}; 
    static Rectangle characterBox = new Rectangle();         
    //Constant values for each level 
    static final int LVTWO_NUM_WALL_HITBOXES = 13; 
    static final int LVONE_NUM_WALL_HITBOXES = 14;
    static final int LVTWO_NUM_SPIKE_HITBOXES = 5; 
    static final int LVTWO_NUM_PLATFORMS = 2; 
    static final Rectangle LVONE_END_POINT = new Rectangle (-1, 0, 1, 287); 
    static final Rectangle LVTWO_END_POINT = new Rectangle (800, 253, 1, 64); 
    //Wall Collision Properties 
    static int[] wallXCoordinates;
    static int[] wallYCoordinates;
    static int[] wallWidths;
    static int [] wallHeights;
    static Rectangle[] wallHitboxes;
    //Spike Collision Properties 
    static int[] spikeXCoordinates;
    static int[] spikeYCoordinates;
    static int[] spikeWidths;
    static int [] spikeHeights;
    static Rectangle[] spikeHitboxes;
    //Read in each saves level and other saving properties
    static int saveOne;
    static int saveTwo;
    static int saveThree; 
    static int level;
    static int currentSave; 
    static Font saveFont = new Font("Arial", Font.BOLD, 76);   
    //Platform properties 
    static BufferedImage levelTwoPlatform;
    static int levelTwoPlatformX = 97;
    static int levelTwoPlatformY = 316; 
    static Rectangle levelTwoPlatformOneBox = new Rectangle(levelTwoPlatformX, levelTwoPlatformY, 31, 15); 
    static boolean movePlatform;
    static int platformVx = 2; 
    //Character movement properties
    static int characterVx; 
    static int characterVy; 
    static final int RUN_SPEED = 3; 
    static final int JUMP_SPEED  = -14; 
    static final int GRAVITY = 1;
    static int jumpCounter = 0; 
    //Main menu properties
    static int menuScreen = 1;  
    static int menuOption = 1; 
    static int saveOption = 1; 
    //Switch properties
    static Rectangle levelTwoSwitch = new Rectangle(675, 231, 27, 19); 
    //Bullet Properties 
    static int numBullets = 6;
    static int[] bulletX = new int[numBullets]; 
    static int[] bulletY = new int[numBullets]; 
    static boolean[] bulletVisible = new boolean[numBullets]; 
    static Rectangle[] bulletHitboxes = new Rectangle[numBullets]; 
    static int bulletW = 2; 
    static int bulletH = 2; 
    static int bulletSpeed = 7;
    static int currentBullet = 0; 
    //flag state properties 
    static boolean inGame = false;
    static boolean gameOver = false; 
    static boolean levelComplete = false; 
    static boolean endGame = false;
    //music properties 
    static AudioInputStream audioStream;
    static Clip music;
    static Clip jump;
    static Clip shoot;
    
//------------------------------------------------------------------------------------
    public static void main(String[] args) throws IOException{
        gameWindow = new JFrame("I Wanna Be The Dude"); 
        gameWindow.setSize(WIDTH,HEIGHT);
        gameWindow.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        
        canvas = new GraphicsPanel();
        canvas.addKeyListener(keyListener);
        gameWindow.add(canvas); 
        
        //Importing all image files
        try {
            levelOneBackground = ImageIO.read(new File("images/levelOnePic.jpg"));
        } catch (IOException ex) {}
        try {                
            levelTwoBackground = ImageIO.read(new File("images/levelTwoPic.jpg"));
        } catch (IOException ex){} 
        try {
            levelTwoPlatform = ImageIO.read(new File("images/brownPlatform.png"));
        } catch (IOException ex){}
        try {
            mainMenu = ImageIO.read(new File("images/mainMenu.png"));
        } catch (IOException ex) {} 
        try {
            controlScreen = ImageIO.read(new File("images/controlScreen.png")); 
        } catch (IOException ex) {} 
        try {
            gameOverScreen = ImageIO.read(new File("images/gameOverScreen.png"));
        }catch (IOException ex) {}
        try {
            levelCompleteScreen = ImageIO.read(new File("images/levelCompleteScreen.png"));
        }catch (IOException ex) {} 
        try { 
            saveSelectScreen = ImageIO.read(new File("images/saveSelectScreen.jpg"));
        }catch (IOException ex) {}
        for (int i=0; i<characterSprites.length; i++){
            try {                
                characterSprites[i] = ImageIO.read(new File("spritePics/character" + Integer.toString(i)+ ".png"));
            } catch (IOException ex){} 
        }
        try{
            endScreen = ImageIO.read(new File("images/endScreen.png"));
        }catch (IOException ex) {}
        
        //Generating bullets 
        Arrays.fill(bulletX, 0); 
        Arrays.fill(bulletY,0);
        Arrays.fill(bulletVisible,false);            
        //reading save numbers into their refrence points 
        saveOne = readSave(1); 
        saveTwo = readSave(2);
        saveThree = readSave(3);
        
        //Playing game song
        try {
            File songFile = new File("gameSong.wav");
            audioStream = AudioSystem.getAudioInputStream(songFile);
            music = AudioSystem.getClip();
            music.open(audioStream);
        } catch (Exception ex){} 
        music.start();
        music.loop(Clip.LOOP_CONTINUOUSLY);
        
        //Importing jumping sound 
        try {
            File jumpFile = new File("jumpSound.wav");
            audioStream = AudioSystem.getAudioInputStream(jumpFile);
            jump = AudioSystem.getClip();
            jump.open(audioStream);
            jump.addLineListener(new JumpListener());
        } catch (Exception ex){} 
        
        //Importing shooting sound 
        try {
            File shootFile = new File("shootSound.wav");
            audioStream = AudioSystem.getAudioInputStream(shootFile);
            shoot = AudioSystem.getClip();
            shoot.open(audioStream);
            shoot.addLineListener(new ShootListener());
        } catch (Exception ex){} 
        
        gameWindow.setVisible(true);
        runGameLoop();
        
    }
//-----------------------------------------------------------------------------------
    public static void runGameLoop() throws IOException{
        while(true){
            //repainting the game every 15 miliseconds 
            gameWindow.repaint();
            try {Thread.sleep(15);} catch(Exception e){}
            
            //Level One
            if (level == 1 && inGame == true && gameOver == false && levelComplete == false){
                //Drawing character into the spawn point
                if (firstSpawn){
                    characterX = lvOneCharacterX;
                    characterY = lvOneCharacterY; 
                    characterBox.setSize(characterW, characterH); 
                    characterBox.setLocation(characterX, characterY);
                    firstSpawn = false; 
                }
                //Seleting the characters picture 
                if (characterState == "standing left"){
                    characterPicNum = 6;
                } else if (characterState == "standing right"){
                    characterPicNum = 7;                              
                } else if (characterState == "walking left"){
                    characterPicNum = nextLeftPic[characterPicNum]; 
                } else if (characterState == "walking right"){                    
                    characterPicNum = nextRightPic[characterPicNum];  
                } else if (characterState == "jumping left"){
                    characterPicNum = 8;
                } else if (characterState == "jumping right"){
                    characterPicNum = 9; 
                }
                //Setting the length of wall hitbox arrays 
                wallXCoordinates = new int [LVONE_NUM_WALL_HITBOXES];
                wallYCoordinates = new int [LVONE_NUM_WALL_HITBOXES];
                wallWidths = new int [LVONE_NUM_WALL_HITBOXES];
                wallHeights = new int [LVONE_NUM_WALL_HITBOXES];
                wallHitboxes = new Rectangle [LVONE_NUM_WALL_HITBOXES];
                
                //Reading wall hitboxes properties, and creating the rectangle objects 
                wallXCoordinates = readWallX(LVONE_NUM_WALL_HITBOXES, level);
                wallYCoordinates = readWallY(LVONE_NUM_WALL_HITBOXES, level); 
                wallWidths = readWallWidths(LVONE_NUM_WALL_HITBOXES, level); 
                wallHeights = readWallHeights(LVONE_NUM_WALL_HITBOXES, level); 
                //Creating an array of rectangles with properties just read
                for(int i = 0; i < LVONE_NUM_WALL_HITBOXES; i++){
                    Rectangle r = new Rectangle(); 
                    r.setSize(wallWidths[i], wallHeights[i]);
                    r.setLocation(wallXCoordinates[i], wallYCoordinates[i]);
                    wallHitboxes[i] = r; 
                }
                //Moving the player horizontally 
                characterX= characterX + characterVx;
                characterBox.setLocation(characterX, characterY); 
                //Handling horizontal collision with walls
                for (int i = 0; i < wallHitboxes.length; i++){
                    if (characterBox.intersects(wallHitboxes[i]) && characterVx <=0){
                        characterX = characterX - characterVx;
                        characterBox.setLocation(characterX, characterY); 
                    }
                }
                for (int i = 0; i < wallHitboxes.length; i++){
                    if (characterBox.intersects(wallHitboxes[i]) && characterVx >=0){
                        characterX = characterX - characterVx;
                        characterBox.setLocation(characterX, characterY); 
                    }
                }//Horiziontal collision end 
                
                //Moving the player vertically 
                //Updating characters vertical velocity 
                if (characterVy < 10){
                    characterVy = characterVy + GRAVITY;
                }
                //Keeping character from falling too fast 
                else { 
                    characterVy = characterVy + 0;
                }
                characterY = characterY + characterVy; 
                characterBox.setLocation(characterX, characterY); 
                
                //Handling vertical collision with walls
                for (int i = 0; i < wallHitboxes.length; i++){
                    if (characterBox.intersects(wallHitboxes[i]) && characterVy < 0){
                        characterY = characterY - characterVy;
                        characterBox.setLocation(characterX, characterY);
                    }
                }
                for (int i = 0; i < wallHitboxes.length; i++){
                    if (characterBox.intersects(wallHitboxes[i]) && characterVy >= 0){
                        characterY = characterY - characterVy;
                        characterBox.setLocation(characterX, characterY);
                        characterVy = 0; 
                        //animation for when character lands 
                        if (characterState == "jumping left"){
                            if (characterVx == 0)
                                characterState = "standing left";
                            else
                                characterState = "walking left";
                        } else if (characterState == "jumping right"){
                            if (characterVx == 0)
                                characterState = "standing right";
                            else
                                characterState = "walking right";
                        }
                        
                        //Resetting characters jump counter 
                        jumpCounter = 0; 
                    }
                }//vertical collision end  
                //Shooting bullets 
                for (int i=0; i<numBullets; i++){
                    if (bulletVisible[i]){
                        bulletX[i] = bulletX[i] + bulletSpeed;
                        //Moving bullet hitbox with the bullet 
                        bulletHitboxes[i].setLocation(bulletX[i], bulletY[i]); 
                        //erasing bullet if it goes off screen  
                        if (bulletX[i]>WIDTH){
                            bulletVisible[i] = false;
                        }
                        //Nested loop to erase a bullet if it hits a wall, and detecting if switch is hit
                        for (int j = 0; j<wallHitboxes.length;j++){
                            if (bulletHitboxes[i].intersects(wallHitboxes[j])){
                                bulletVisible[i] = false;
                            }
                            if (bulletHitboxes[i].intersects(levelTwoSwitch)){
                                //Erasing bullet and activating platforms 
                                bulletVisible[i] = false; 
                                movePlatform = true; 
                            }
                        }
                    }
                }//Bullet properties end 
                
                //Game over if the player goes below the screen 
                if (characterY > HEIGHT){
                    gameOver = true; 
                }
                //Checking if player reached the end of the level 
                if (characterBox.intersects(LVONE_END_POINT)){
                    levelComplete = true;
                }
            }//level one end 
            
            //Level two 
            if (level == 2 && inGame == true && gameOver == false && levelComplete == false){
                //Setting the length of wall and spike hitbox arrays 
                wallXCoordinates = new int [LVTWO_NUM_WALL_HITBOXES];
                wallYCoordinates = new int [LVTWO_NUM_WALL_HITBOXES];
                wallWidths = new int [LVTWO_NUM_WALL_HITBOXES];
                wallHeights = new int [LVTWO_NUM_WALL_HITBOXES];
                wallHitboxes = new Rectangle [LVTWO_NUM_WALL_HITBOXES];
                spikeXCoordinates = new int [LVTWO_NUM_SPIKE_HITBOXES];
                spikeYCoordinates = new int [LVTWO_NUM_SPIKE_HITBOXES];
                spikeWidths = new int [LVTWO_NUM_SPIKE_HITBOXES];
                spikeHeights = new int [LVTWO_NUM_SPIKE_HITBOXES];
                spikeHitboxes = new Rectangle [LVTWO_NUM_SPIKE_HITBOXES];
               
                //Drawing character into the spawn point
                if (firstSpawn){
                    characterX = lvTwoCharacterX;
                    characterY = lvTwoCharacterY; 
                    characterBox.setSize(characterW, characterH); 
                    characterBox.setLocation(characterX, characterY);
                    firstSpawn = false; 
                }
                //Seleting the characters picture 
                if (characterState == "standing left"){
                    characterPicNum = 6;
                } else if (characterState == "standing right"){
                    characterPicNum = 7;                              
                } else if (characterState == "walking left"){
                    characterPicNum = nextLeftPic[characterPicNum]; 
                } else if (characterState == "walking right"){                    
                    characterPicNum = nextRightPic[characterPicNum];  
                } else if (characterState == "jumping left"){
                    characterPicNum = 8;
                } else if (characterState == "jumping right"){
                    characterPicNum = 9; 
                }
                
                //Reading wall hitboxes properties, and creating the rectangle objects 
                wallXCoordinates = readWallX(LVTWO_NUM_WALL_HITBOXES, level);
                wallYCoordinates = readWallY(LVTWO_NUM_WALL_HITBOXES, level); 
                wallWidths = readWallWidths(LVTWO_NUM_WALL_HITBOXES, level); 
                wallHeights = readWallHeights(LVTWO_NUM_WALL_HITBOXES, level); 
                //Creating an array of rectangles with properties just read
                for(int i = 0; i < LVTWO_NUM_WALL_HITBOXES; i++){
                    Rectangle r = new Rectangle(); 
                    r.setSize(wallWidths[i], wallHeights[i]);
                    r.setLocation(wallXCoordinates[i], wallYCoordinates[i]);
                    wallHitboxes[i] = r; 
                }
                //Reading spike hitbox properties, and creating the rectangle objects 
                spikeXCoordinates = readSpikeX(LVTWO_NUM_SPIKE_HITBOXES, level); 
                spikeYCoordinates = readSpikeY(LVTWO_NUM_SPIKE_HITBOXES, level); 
                spikeWidths = readSpikeWidths(LVTWO_NUM_SPIKE_HITBOXES, level); 
                spikeHeights = readSpikeHeights(LVTWO_NUM_SPIKE_HITBOXES, level); 
                //Creating an array of rectangles with the properties just read 
                for(int i = 0; i < LVTWO_NUM_SPIKE_HITBOXES; i++){
                    Rectangle r = new Rectangle(); 
                    r.setSize(spikeWidths[i], spikeHeights[i]);
                    r.setLocation(spikeXCoordinates[i], spikeYCoordinates[i]);
                    spikeHitboxes[i] = r; 
                }
                
                //Moving the player horizontally 
                characterX= characterX + characterVx;
                characterBox.setLocation(characterX, characterY); 
                //Handling horizontal collision with walls
                for (int i = 0; i < wallHitboxes.length; i++){
                    if (characterBox.intersects(wallHitboxes[i]) && characterVx <=0){
                        characterX = characterX - characterVx;
                        characterBox.setLocation(characterX, characterY); 
                    }
                }
                for (int i = 0; i < wallHitboxes.length; i++){
                    if (characterBox.intersects(wallHitboxes[i]) && characterVx >=0){
                        characterX = characterX - characterVx;
                        characterBox.setLocation(characterX, characterY); 
                    }
                }//Horiziontal collision end 
                
                //Moving the player vertically 
                //Updating characters vertical velocity 
                if (characterVy < 10){
                    characterVy = characterVy + GRAVITY;
                }
                //Keeping character from falling too fast 
                else { 
                    characterVy = characterVy + 0;
                }
                characterY = characterY + characterVy; 
                characterBox.setLocation(characterX, characterY); 
                
                //making sure the character doesnt go off the left 
                if (characterX <= LEFT){
                    characterX = characterX - characterVx; 
                } 
                //Handling vertical collision with walls
                for (int i = 0; i < wallHitboxes.length; i++){
                    if (characterBox.intersects(wallHitboxes[i]) && characterVy < 0){
                        characterY = characterY - characterVy;
                        characterBox.setLocation(characterX, characterY);
                    }
                }
                for (int i = 0; i < wallHitboxes.length; i++){
                    if (characterBox.intersects(wallHitboxes[i]) && characterVy >= 0){
                        characterY = characterY - characterVy;
                        characterBox.setLocation(characterX, characterY);
                        characterVy = 0; 
                        //animation for when the character lands 
                        if (characterState == "jumping left"){
                            if (characterVx == 0)
                                characterState = "standing left";
                            else
                                characterState = "walking left";
                        } else if (characterState == "jumping right"){
                            if (characterVx == 0)
                                characterState = "standing right";
                            else
                                characterState = "walking right";
                        }
                        //Resetting characters jump counter 
                        jumpCounter = 0; 
                    }
                }//vertical collision end  
                
                //Moving platforms if player hit switch 
                if (movePlatform == true){
                    levelTwoPlatformX = levelTwoPlatformX + platformVx;
                }
                //Adjusting platform hitboxes to match platforms
                levelTwoPlatformOneBox.setLocation(levelTwoPlatformX, levelTwoPlatformY); 
                
                //Collision between player and moving platforms 
                if (characterBox.intersects(levelTwoPlatformOneBox) && characterVx <=0 && characterY > 331 && characterY +characterH < 316){
                    characterX = characterX - characterVx;
                    characterBox.setLocation(characterX, characterY); 
                }
                else if (characterBox.intersects(levelTwoPlatformOneBox) && characterVx >=0 && characterY > 331 && characterY +characterH < 316){
                    characterX = characterX - characterVx;
                    characterBox.setLocation(characterX, characterY); 
                }
                else if (characterBox.intersects(levelTwoPlatformOneBox) && characterVy <= 0){
                    characterY = characterY - characterVy;
                    characterBox.setLocation(characterX, characterY);
                    characterVy = 0;
                }
                else if (characterBox.intersects(levelTwoPlatformOneBox) && characterVy >= 0){
                    characterY = characterY - characterVy;
                    characterBox.setLocation(characterX, characterY);
                    //Resetting jump counter 
                    jumpCounter = 0;
                }
                //Changing direction of the platforms if they reach a certain point
                if (levelTwoPlatformX + 31 >= 318){
                    platformVx = -platformVx;
                }
                if (levelTwoPlatformX < 97){
                    platformVx = -platformVx; 
                }
                
                //Detecting collision between player and spikes 
                for(int i = 0; i < spikeHitboxes.length; i++){
                    if(characterBox.intersects(spikeHitboxes[i])){
                        gameOver = true; 
                    }
                }
                //Shooting bullets 
                for (int i=0; i<numBullets; i++){
                    if (bulletVisible[i]){
                        bulletX[i] = bulletX[i] + bulletSpeed;
                        //Moving bullet hitbox with the bullet 
                        bulletHitboxes[i].setLocation(bulletX[i], bulletY[i]); 
                        //erasing bullet if it goes off screen  
                        if (bulletX[i]>WIDTH){
                            bulletVisible[i] = false;
                        }
                        //Nested loop to erase a bullet if it hits a wall, and detecting if switch is hit
                        for (int j = 0; j<wallHitboxes.length;j++){
                            if (bulletHitboxes[i].intersects(wallHitboxes[j])){
                                bulletVisible[i] = false;
                            }
                            if (bulletHitboxes[i].intersects(levelTwoSwitch)){
                                //Erasing bullet and activating platforms 
                                bulletVisible[i] = false; 
                                movePlatform = true; 
                            }
                        }
                    }
                }//Bullet properties end 
                
                //Game over if the player goes below the screen 
                if (characterY > HEIGHT){
                    gameOver = true; 
                }
                //ending game if player reaches end of screen 
                if (characterBox.intersects(LVTWO_END_POINT)){
                    endGame = true; 
                }
            }//Level two end 
        }
    } 
//-----------------------------------------------------------------------------------
    static class GraphicsPanel extends JPanel {
        public GraphicsPanel(){
            setFocusable(true); 
            requestFocusInWindow();
        }
        public void paintComponent(Graphics g){ 
            super.paintComponent(g);  
            if (inGame == false && menuScreen == 1){
                //Drawing the main menu 
                g.drawImage(mainMenu, LEFT, TOP, this);
                //Drawing a rectangle over the current option the user is on 
                if (menuOption == 1){
                    g.setColor(Color.yellow);
                    g.drawRect(24, 280, 158, 76);
                }
                else if (menuOption == 2){
                    g.setColor(Color.yellow);
                    g.drawRect(240,279, 320, 63); 
                }
                else { 
                    g.setColor(Color.yellow);
                    g.drawRect(601 ,274, 168, 65);
                }
            }
            else if (inGame == false && menuScreen == 2){ 
                //Drawing the controls screen 
                g.drawImage (controlScreen, LEFT, TOP, this);
            }
            else if (inGame == false && menuScreen == 3){
                g.drawImage (saveSelectScreen, LEFT, TOP, this);
                //Drawing the level number each save file is on 
                g.setColor(Color.yellow);
                g.setFont(saveFont); 
                g.drawString(Integer.toString(saveOne), 85, 444);
                g.drawString(Integer.toString(saveTwo), 382, 444);
                g.drawString(Integer.toString(saveThree),  650, 444);
                //Drawing a rectangle over whichever save file user is on 
                if (saveOption == 1){
                    g.setColor(Color.yellow);
                    g.drawRect(6, 199, 216, 271);
                }
                else if (saveOption == 2){
                    g.setColor(Color.yellow); 
                    g.drawRect(291, 195, 225, 271);
                }
                else if (saveOption == 3){
                    g.setColor(Color.yellow); 
                    g.drawRect(563, 183, 225, 271);
                }
            }
            else if (level == 1 && inGame == true && gameOver == false && levelComplete == false){
                //Drawing the level one image 
                g.drawImage(levelOneBackground, LEVEL_X, LEVEL_Y - 30, this);
                
                //Drawing the character
                g.drawImage(characterSprites[characterPicNum], characterX, characterY, this);
                //Drawing bullets
                g.setColor(Color.white);
                for (int i=0; i<numBullets; i++){
                    if (bulletVisible[i])
                        g.fillOval(bulletX[i],bulletY[i],bulletW,bulletH);
                }
            }
            else if (level == 2 && inGame == true && gameOver == false && endGame == false){
                //Drawing the level images depending on what level the user is on 
                g.drawImage(levelTwoBackground, LEVEL_X, LEVEL_Y, this);
                //Drawing platforms and adjusting their values if they are moving 
                g.drawImage(levelTwoPlatform, levelTwoPlatformX, levelTwoPlatformY, this);
                //Drawing the character 
                g.drawImage(characterSprites[characterPicNum], characterX, characterY, this);
                //Drawing bullets
                g.setColor(Color.white);
                for (int i=0; i<numBullets; i++){
                    if (bulletVisible[i])
                        g.fillOval(bulletX[i],bulletY[i],bulletW,bulletH);
                }
            }
            else if (inGame == true && gameOver == true){
                //Drawing game over screen 
                g.drawImage(gameOverScreen, LEFT, TOP, this);
            }
            else if (inGame == true && levelComplete == true){
                //Drawing level complete screen 
                g.drawImage(levelCompleteScreen, LEFT, TOP, this);
            }
            else if (endGame == true){
                g.drawImage(endScreen, LEFT, TOP, this); 
            }
        }
    }
//------------------------------------------------------------------------------------    
    static class MyKeyListener implements KeyListener{
        public void keyPressed(KeyEvent e){
            int key = e.getKeyCode();
            if (key == KeyEvent.VK_LEFT){
                //Determining if the player is on the menu or in game
                if (inGame == false && menuScreen == 1){
                    //Main menu interactions 
                    if (menuOption == 1 && menuScreen == 1){
                        menuOption = 3; 
                    }
                    else { 
                        menuOption = menuOption - 1;
                    }
                }
                else if (inGame == false && menuScreen == 3){
                    //Save screen interactions
                    if (saveOption == 1 && menuScreen == 3){
                        saveOption = 3;
                    }
                    else {
                        saveOption = saveOption - 1;
                    }
                }
                else if (inGame == true && gameOver == false && levelComplete == false){
                    //All character animation and movement
                    if (characterState == "standing left"){
                        characterState = "walking left";
                        characterVx = -RUN_SPEED;
                    } else if (characterState == "standing right"){
                        characterState = "walking left";
                        characterVx = -RUN_SPEED;
                    } else if (characterState == "walking left"){
                        characterVx = -RUN_SPEED;
                    } else if (characterState == "walking right"){
                        characterState = "walking left";
                        characterVx = -RUN_SPEED;
                    } else if (characterState == "jumping left"){
                        characterVx = -RUN_SPEED;
                    } else if (characterState == "jumping right"){
                        characterState = "jumping left";
                        characterVx =RUN_SPEED;
                    }
                }
            }
            if (key == KeyEvent.VK_RIGHT){
                //Determining if the player is on menu or in game 
                if (inGame == false && menuScreen == 1){
                    //Main menu interactions 
                    if (menuOption == 3){
                        menuOption = 1;
                    }
                    else {
                        menuOption = menuOption + 1;
                    }
                }
                else if (inGame == false && menuScreen == 3){
                    //Save screen interactions
                    if (saveOption == 3 && menuScreen == 3){
                        saveOption = 1;
                    }
                    else {
                        saveOption = saveOption + 1;
                    }
                }
                else if (inGame == true && gameOver == false && levelComplete == false){ 
                    //Giving the character positive (right) horizontal speed and animation
                    if (characterState == "standing left"){
                        characterState = "walking right";
                        characterVx = RUN_SPEED;
                    } else if (characterState == "standing right"){
                        characterState = "walking right";
                        characterVx = RUN_SPEED;
                    } else if (characterState == "walking left"){
                        characterState = "walking left";
                        characterVx = RUN_SPEED;
                    } else if (characterState == "walking right"){
                        characterVx = RUN_SPEED;
                    } else if (characterState == "jumping left"){
                        characterState = "jumping right";
                        characterVx = RUN_SPEED;
                    } else if (characterState == "jumping right"){
                        characterVx = RUN_SPEED;
                    }
                }
            }
        
            if (key == KeyEvent.VK_UP){
                //Making sure character cant jump more than twice 
                if (jumpCounter <= 1 && inGame == true){
                    characterVy = JUMP_SPEED;
                    jumpCounter = jumpCounter + 1; 
                    //animations 
                    if (characterState == "standing left"){
                        characterState = "jumping left";
                        characterVy = JUMP_SPEED;
                    } else if (characterState == "standing right"){
                        characterState = "jumping right";
                        characterVy = JUMP_SPEED;
                    } else if (characterState == "walking left"){
                        characterState = "jumping left";
                        characterVy = JUMP_SPEED;
                    } else if (characterState == "walking right"){
                        characterState = "jumping right";
                        characterVy = JUMP_SPEED;
                    } else if (characterState == "jumping left"){
                        characterVy = JUMP_SPEED; 
                    } else if (characterState == "jumping right"){
                        characterVy = JUMP_SPEED;
                    }
                    // play the sound effect        
                    if (jump.isRunning()){
                        jump.stop();              
                        jump.flush();              
                        jump.setFramePosition(0);  
                    }
                    jump.start();
                }
            }
            
            if (key == KeyEvent.VK_ENTER){
                //All menu interactions for enter button 
                if (inGame == false && menuScreen == 1){
                    //Determining what button on the main menu the user is clicking
                    if (menuOption == 1){
                        menuScreen = 3;
                    }
                    else if (menuOption == 2){
                        menuScreen = 2;
                    }
                    else { 
                        System.exit(0);
                    }
                }
                else if (inGame == false && menuScreen == 2){
                    //returning from control screen to main menu 
                    menuScreen = 1;
                }
                else if (inGame == false && menuScreen == 3){
                    //Selecting a save file, setting level to save files level and saving the number of the chosen save
                    if (saveOption == 1){
                        currentSave = 1;
                        level = saveOne; 
                        inGame = true; 
                    }
                    else if (saveOption == 2){
                        currentSave = 2; 
                        level = saveTwo; 
                        inGame = true;
                    }
                    else if (saveOption == 3){
                        currentSave = 3; 
                        level = saveThree; 
                        inGame = true;
                    }
                }
                else if (inGame == true && gameOver == true){
                    //Redraw character at start of screen, reset all interactions 
                    movePlatform = false; 
                    firstSpawn = true;
                    gameOver = false;
                }
                else if (inGame == true && levelComplete == true){
                    //Moving onto next level if player is on level one
                    if (level == 1){
                        level = 2;
                        firstSpawn = true;
                        levelComplete = false;
                        //Saving the players progress to their save file 
                        try { writeSave(currentSave, level); }catch (IOException ex){}
                    }
                }
                else if (endGame == true){
                    inGame = false; 
                    menuScreen = 1; 
                    endGame = false;
                }
            }
            if (key == KeyEvent.VK_Z){
                //Checking if the user is currently in game 
                if (inGame == true){
                    //Shooting bullets, assign bullet coordinates to where player is 
                    bulletX[currentBullet] = characterX;
                    bulletY[currentBullet] = characterY;
                    //Creating a hitbox for the shot bullet
                    Rectangle bullet = new Rectangle();
                    bullet.setSize(bulletW, bulletH);
                    bullet.setLocation(bulletX[currentBullet], bulletY[currentBullet]); 
                    bulletHitboxes[currentBullet] = bullet;
                    //Displaying the bullet and moving to the next bullet in the array 
                    bulletVisible[currentBullet] = true;
                    currentBullet = (currentBullet + 1)%numBullets;
                    //playing shooting sound
                    if (shoot.isRunning()){
                        shoot.stop();              
                        shoot.flush();              
                        shoot.setFramePosition(0);  
                    }
                    shoot.start();
                }
            }
            if (key == KeyEvent.VK_ESCAPE){
                if (inGame == true){
                    inGame = false; 
                    menuScreen = 1; 
                    endGame = false;
                }
            }
        }
        
        public void keyReleased(KeyEvent e){
            int key = e.getKeyCode();
            if (key == KeyEvent.VK_LEFT){ 
                //Giving character idle animation 
                if (characterState == "standing left" || characterState == "walking left" || characterState == "standing right" || characterState == "walking right"){
                    characterState = "standing left"; 
                }
                //stoping character 
                characterVx = 0;
            }
            if (key == KeyEvent.VK_RIGHT){
                //giving character idle animation
                if (characterState == "standing left" || characterState == "walking left" || characterState == "standing right" || characterState == "walking right"){
                    characterState = "standing right"; 
                }
                //stopping character
                characterVx = 0; 
            }
            if (key == KeyEvent.VK_UP){
            }
        }       
        public void keyTyped(KeyEvent e){
        }        
    }// MyKeyListener class end
//------------------------------------------------------------------------------------    
    //All methods for reading in and creating level hitboxes  
    public static int[] readWallX(int numCoordinates,int level) throws IOException{ 
        //Creating array for coordinates 
        int[] coordinates = new int[numCoordinates]; 
        //Determining which level to read the coordinates for 
        String fileName = "level" + Integer.toString(level) +  "WallX.txt";           
        //Determining which level to read the coordinates for 
        File xCoordinates = new File(fileName); 
        Scanner readCoordinates = new Scanner(xCoordinates); 
        //Reading all coordinates into an array 
        for (int i = 0; i < numCoordinates; i++){
            coordinates[i] = Integer.parseInt(readCoordinates.nextLine());
        }
        readCoordinates.close();
        return coordinates; 
    }
//------------------------------------------------------------------------------------    
    public static int[] readWallY(int numCoordinates,int level) throws IOException{
        //Creating array for coordinates 
        int[] coordinates = new int[numCoordinates]; 
        //Determining which level to read the coordinates for 
        String fileName = "level" + Integer.toString(level) +  "WallY.txt";
        //Determining which level to read the coordinates for 
        File yCoordinates = new File(fileName);  
        Scanner readCoordinates = new Scanner(yCoordinates);          
        //Reading all coordinates into an array 
        for (int i = 0; i < numCoordinates; i++){
            coordinates[i] = Integer.parseInt(readCoordinates.nextLine());
        }
        readCoordinates.close();
        return coordinates; 
    }
//------------------------------------------------------------------------------------    
    public static int[] readWallWidths(int numWidths,int level) throws IOException{
        //Creating array for coordinates 
        int[] widths = new int[numWidths]; 
        //Variable to determine which levels properties to get 
        String fileName = "level" + Integer.toString(level) +  "WallW.txt";
        //Determining which level to read the coordinates for 
        File levelWidths = new File(fileName); 
        Scanner readWidths = new Scanner(levelWidths); 
        //Reading all coordinates into an array 
        for (int i = 0; i < numWidths; i++){
            widths[i] = Integer.parseInt(readWidths.nextLine());
        }
        readWidths.close();
        return widths; 
    }
//------------------------------------------------------------------------------------    
    public static int[] readWallHeights(int numHeights,int level) throws IOException{
        //Creating array for coordinates 
        int[] heights = new int[numHeights]; 
        //Determining which level to read the coordinates for 
        String fileName = "level" + Integer.toString(level) +  "WallH.txt";
        //Determining which level to read the coordinates for 
        File levelHeights = new File(fileName); 
        Scanner readHeights = new Scanner(levelHeights);  
        //Reading all coordinates into an array 
        for (int i = 0; i < numHeights; i++){
            heights[i] = Integer.parseInt(readHeights.nextLine());
        }
        readHeights.close();
        return heights; 
    }
//------------------------------------------------------------------------------------    
    public static int[] readSpikeX(int numCoordinates,int level) throws IOException{
        //Creating array for coordinates 
        int[] coordinates = new int[numCoordinates]; 
        //Determining which level to read the coordinates for 
        String fileName = "level" + Integer.toString(level) +  "SpikeX.txt";
        //Determining which level to read the coordinates for 
        File xSpikes = new File(fileName); 
        Scanner readCoordinates = new Scanner(xSpikes);  
        //Reading all coordinates into an array 
        for (int i = 0; i < numCoordinates; i++){
            coordinates[i] = Integer.parseInt(readCoordinates.nextLine());
        }
        readCoordinates.close();
        return coordinates; 
    }
//------------------------------------------------------------------------------------    
    public static int[] readSpikeY(int numCoordinates,int level) throws IOException{
        //Creating array for coordinates 
        int[] coordinates = new int[numCoordinates]; 
        //Determining which level to read the coordinates for 
        String fileName = "level" + Integer.toString(level) +  "SpikeY.txt";
        //Determining which level to read the coordinates for 
        File ySpikes = new File(fileName); 
        Scanner readCoordinates = new Scanner(ySpikes);  
        //Reading all coordinates into an array 
        for (int i = 0; i < numCoordinates; i++){
            coordinates[i] = Integer.parseInt(readCoordinates.nextLine());
        }
        readCoordinates.close();
        return coordinates; 
    }
//------------------------------------------------------------------------------------    
    public static int[] readSpikeWidths(int numWidths,int level) throws IOException{
        //Creating array for coordinates 
        int[] widths = new int[numWidths]; 
        //Determining which level to read the coordinates for 
        String fileName = "level" + Integer.toString(level) +  "SpikeW.txt";
        //Determining which level to read the coordinates for 
        File wSpikes = new File(fileName); 
        Scanner readWidths = new Scanner(wSpikes);  
        //Reading all coordinates into an array 
        for (int i = 0; i < numWidths; i++){
            widths[i] = Integer.parseInt(readWidths.nextLine());
        }
        readWidths.close();
        return widths; 
    }    
//------------------------------------------------------------------------------------        
    public static int[] readSpikeHeights(int numHeights,int level) throws IOException{
        //Creating array for coordinates 
        int[] heights = new int[numHeights]; 
        //Determining which level to read the coordinates for 
        String fileName = "level" + Integer.toString(level) +  "SpikeH.txt";
        //Determining which level to read the coordinates for 
        File hSpikes = new File(fileName); 
        Scanner readHeights = new Scanner(hSpikes);  
        //Reading all coordinates into an array 
        for (int i = 0; i < numHeights; i++){
            heights[i] = Integer.parseInt(readHeights.nextLine());
        }
        readHeights.close();
        return heights; 
    }
//------------------------------------------------------------------------------------            
    public static int readSave(int saveNum) throws IOException{
        //Determining which save file to get level from 
        String fileName = "file" + Integer.toString(saveNum) + "Level.txt";
        //Creating a scanner to read the file 
        File save = new File(fileName);
        Scanner readSave = new Scanner(save);
        //reading the level number into a refrence point
        int level = Integer.parseInt(readSave.nextLine());
        readSave.close(); 
        return level; 
    }
//------------------------------------------------------------------------------------        
    public static void writeSave(int currentSave, int level) throws IOException{
        //determining which file to write to 
        String fileName = "file" + Integer.toString(currentSave) + "Level.txt";
        //printwriter to write into file
        File save = new File(fileName);
        PrintWriter pw = new PrintWriter(save);
        //writing to file 
        pw.println(level); 
        pw.close();
    }
//------------------------------------------------------------------------------------
    //Methods for stopping different sounds
    static class JumpListener implements LineListener {
        public void update(LineEvent event) {
            if (event.getType() == LineEvent.Type.STOP) {
                jump.flush();          
                jump.setFramePosition(0);  
            }
        }
    } 
//------------------------------------------------------------------------------------        
    static class ShootListener implements LineListener {
        public void update(LineEvent event) {
            if (event.getType() == LineEvent.Type.STOP) {
                shoot.flush();          
                shoot.setFramePosition(0);  
            }
        }
    }
}