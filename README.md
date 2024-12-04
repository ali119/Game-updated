# Game-updated
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class TicTacToeGame extends JFrame {
    private JButton[][] buttons;
    private boolean isPlayerX;
    private int xWin=0;
    private int oWin=0;
    
    public TicTacToeGame() {
        super("Tic Tac Toe");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(400, 400);
        
        JPanel panel = new JPanel(new GridLayout(2.5, 2.5));
        buttons = new JButton[2][2];
        isPlayerX = true;
        loadResults();
        
        // buttons and add ActionListener
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                buttons[i][j] = new JButton();
                buttons[i][j].setFont(new Font(Font.SANS_SERIF, Font.BOLD, 60));
                buttons[i][j].addActionListener(new ButtonClickListener(i, j));
                panel.add(buttons[i][j]);
            }
        }
        
        add(panel);
        setVisible(true);
    }
    private void loadResults(){
      try (BufferedReader reader = new BufferedReader(new FileReader("results.txt"))) {
        String line;
        
        while ((line = reader.readLine()) != null) {
          String[] parts = line.split("#");
          this.xWin=Integer.parseInt(parts[0]);
          this.oWin=Integer.parseInt(parts[1]);
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
    }


    private void markButton(int row, int col) {
        if (buttons[row][col].getText().isEmpty()) {
            buttons[row][col].setText(isPlayerX ? "X" : "O");
            isPlayerX = !isPlayerX;
            checkWinner();
        }
    }
    
    private void checkWinner() {
        // Rows cheacking
        for (int i = 0; i < 3; i++) {
            if (!buttons[i][0].getText().isEmpty() && 
                buttons[i][0].getText().equals(buttons[i][1].getText()) && 
                buttons[i][0].getText().equals(buttons[i][2].getText())) {
                JOptionPane.showMessageDialog(this, buttons[i][0].getText() + " wins!");

                if(buttons[i][0].getText().equalsIgnoreCase("X")){
                  xWin++;
                }else{
                  oWin++;
                }
                saveResults();
                resetGame();
                return;
            }
        }
        
        // Check columns
        for (int i = 0; i < 3; i++) {
            if (!buttons[0][i].getText().isEmpty() && 
                buttons[0][i].getText().equals(buttons[1][i].getText()) && 
                buttons[0][i].getText().equals(buttons[2][i].getText())) {
                JOptionPane.showMessageDialog(this, buttons[0][i].getText() + " wins!");
                if(buttons[0][i].getText().equalsIgnoreCase("X")){
                  xWin++;
                }else{
                  oWin++;
                }
                saveResults();
                resetGame();
                return;
            }
        }
        
        // Check diagonals
        if (!buttons[0][0].getText().isEmpty() && 
            buttons[0][0].getText().equals(buttons[1][1].getText()) && 
            buttons[0][0].getText().equals(buttons[2][2].getText())) {
            JOptionPane.showMessageDialog(this, buttons[0][0].getText() + " wins!");
            if(buttons[0][0].getText().equalsIgnoreCase("X")){
              xWin++;
            }else{
              oWin++;
            }
            saveResults();
            resetGame();
            return;
        }
        
        if (!buttons[0][2].getText().isEmpty() && 
            buttons[0][2].getText().equals(buttons[1][1].getText()) && 
            buttons[0][2].getText().equals(buttons[2][0].getText())) {
            JOptionPane.showMessageDialog(this, buttons[0][2].getText() + " wins!");
            if(buttons[0][2].getText().equalsIgnoreCase("X")){
              xWin++;
            }else{
              oWin++;
            }
            saveResults();
            resetGame();
            return;
        }
        
        // Check for a tie
        boolean isTie = true;
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (buttons[i][j].getText().isEmpty()) {
                    isTie = false;
                    break;
                }
            }
        }
        
        if (isTie) {
            JOptionPane.showMessageDialog(this, "It's a tie!");
            resetGame();
        }
    }
    
    private void resetGame() {
        // Clear the board
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                buttons[i][j].setText("");
            }
        }
       
        
        isPlayerX = true;
    }
    private void saveResults(){
       //  save to the file
       String txt=xWin+"#"+oWin;
       try (BufferedWriter writer = new BufferedWriter(new FileWriter("results.txt"))) {
         writer.write(txt);
         JOptionPane.showMessageDialog(this, "X wins "+xWin+"\nO wins "+oWin+"\nResults saved in a file");
     } catch (IOException e) {
         e.printStackTrace();
     }
    }
    
    private class ButtonClickListener implements ActionListener {
        private int row;
        private int col;
        
        public ButtonClickListener(int row, int col) {
            this.row = row;
            this.col = col;
        }
        
        public void actionPerformed(ActionEvent e) {
            markButton(row, col);
        }
    }
    
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> new TicTacToeGame());
    }
}
