# SnakeGame 
/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 */

package com.mycompany.snakegame;

/**
 *
 * @author Montefalcon 
 */
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.Random;

public class SnakeGame extends JPanel implements ActionListener {

    private static final int BOARD_WIDTH = 600;
    private static final int BOARD_HEIGHT = 600;
    private static final int DOT_SIZE = 20;
    private static final int ALL_DOTS = (BOARD_WIDTH * BOARD_HEIGHT) / (DOT_SIZE * DOT_SIZE);
    private static final int RAND_POS = 29;
    private static final int DELAY = 120;

    private final int x[] = new int[ALL_DOTS];
    private final int y[] = new int[ALL_DOTS];

    private int dots;
    private int apple_x;
    private int apple_y;

    private boolean leftDirection = false;
    private boolean rightDirection = true;
    private boolean upDirection = false;
    private boolean downDirection = false;
    private boolean inGame = true;

    private Timer timer;
    private int score = 0;

    public SnakeGame() {
        initBoard();
    }

    private void initBoard() {
        setBackground(Color.WHITE);  // Changed background to white
        setFocusable(true);
        setPreferredSize(new Dimension(BOARD_WIDTH, BOARD_HEIGHT));
        addKeyListener(new TAdapter());
        initGame();
    }

    private void initGame() {
        dots = 3; // Initial snake length

        for (int i = 0; i < dots; i++) {
            x[i] = 100 - i * DOT_SIZE;
            y[i] = 100;
        }

        locateApple();

        timer = new Timer(DELAY, this);
        timer.start();
    }

    @Override
    public void paintComponent(Graphics g) {
        super.paintComponent(g);
        draw(g);
    }

    private void draw(Graphics g) {
        if (inGame) {
            // Draw apple (food)
            g.setColor(Color.BLACK); // changed apple color to black
            g.fillOval(apple_x, apple_y, DOT_SIZE, DOT_SIZE);

            for (int i = 0; i < dots; i++) {
                if (i == 0) {
                    g.setColor(new Color(200, 0, 0)); // Head in bright red
                    g.fillRect(x[i], y[i], DOT_SIZE, DOT_SIZE);
                } else {
                    g.setColor(new Color(150, 0, 0)); // Body in darker red
                    g.fillRect(x[i], y[i], DOT_SIZE, DOT_SIZE);
                }
            }

            String scoreMsg = "Score: " + score;
            g.setColor(Color.BLACK);
            g.setFont(new Font("Arial", Font.BOLD, 18));
            FontMetrics fm = getFontMetrics(g.getFont());
            g.drawString(scoreMsg, BOARD_WIDTH - fm.stringWidth(scoreMsg) - 10, 25);

        } else {
            gameOver(g);
        }
    }

    private void gameOver(Graphics g) {
        String msg = "Game Over";
        String scoreMsg = "Your score: " + score;
        String restartMsg = "Press SPACE to Restart";
        g.setColor(Color.BLACK);
        g.setFont(new Font("Arial", Font.BOLD, 48));
        FontMetrics fm = getFontMetrics(g.getFont());
        g.drawString(msg, (BOARD_WIDTH - fm.stringWidth(msg)) / 2, BOARD_HEIGHT / 3);

        g.setFont(new Font("Arial", Font.PLAIN, 24));
        fm = getFontMetrics(g.getFont());
        g.drawString(scoreMsg, (BOARD_WIDTH - fm.stringWidth(scoreMsg)) / 2, BOARD_HEIGHT / 2);

        g.drawString(restartMsg, (BOARD_WIDTH - fm.stringWidth(restartMsg)) / 2, BOARD_HEIGHT / 2 + 40);
    }

    private void checkApple() {
        if ((x[0] == apple_x) && (y[0] == apple_y)) {
            dots++;
            score++;
            locateApple();
        }
    }

    private void move() {
        for (int i = dots - 1; i > 0; i--) {
            x[i] = x[i - 1];
            y[i] = y[i - 1];
        }

        if (leftDirection) {
            x[0] -= DOT_SIZE;
        }

        if (rightDirection) {
            x[0] += DOT_SIZE;
        }

        if (upDirection) {
            y[0] -= DOT_SIZE;
        }

        if (downDirection) {
            y[0] += DOT_SIZE;
        }
    }

    private void checkCollision() {
        // Check self collision
        for (int i = dots - 1; i > 0; i--) {
            if ((x[0] == x[i]) && (y[0] == y[i])) {
                inGame = false;
            }
        }

        // Check collision with walls
        if (x[0] < 0) {
            inGame = false;
        }

        if (x[0] >= BOARD_WIDTH) {
            inGame = false;
        }

        if (y[0] < 0) {
            inGame = false;
        }

        if (y[0] >= BOARD_HEIGHT) {
            inGame = false;
        }

        if (!inGame) {
            timer.stop();
        }
    }

    private void locateApple() {
        int r = (int) (Math.random() * RAND_POS);
        apple_x = ((r * DOT_SIZE));

        r = (int) (Math.random() * RAND_POS);
        apple_y = ((r * DOT_SIZE));
    }

    @Override
    public void ac
