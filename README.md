package org.example;

import javax.swing.event.DocumentEvent;
import javax.swing.event.DocumentListener;
import java.awt.*;
import java.awt.event.ActionEvent;
import javax.swing.*;
import java.awt.event.ActionListener;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.PrintStream;
import java.util.Random;

public class Main {
    public static void main(String[] args) {

        JButton button = new JButton("Check");
        button.setBackground(Color.GREEN);


        JButton button2 = new JButton("Шифровать");
        button2.setBackground(Color.RED);


        JButton button3 = new JButton("Расшифровать");
        button3.setBackground(Color.red);


        JFrame frame = new JFrame("Enigma(not)");
        frame.setSize(900, 400);
        frame.setLayout(new FlowLayout()); 

        JCheckBox jCheckBox = new JCheckBox("Я соглашаюсь что вадосик абебр чад");

        frame.add(jCheckBox);
        frame.add(button);
        frame.add(button2);
        frame.add(button3);
        frame.setVisible(true);
        button2.setEnabled(false);
        button3.setEnabled(false);
        button.setEnabled(false);


        JTextField textField = new JTextField("Для использования введите букву, которую нужно зашифровать, вводить надо по 1 букве и нажать на кнопку шифровать , она сохранит шифр в файл, для сохранения файла надо создать папку codes.txt, её путь представляет собой C:\\Users\\Win10\\Desktop\\codes.txt", 100);
        textField.setEditable(false);
        frame.add(textField);


        String[] symbols = {"а", "б", "в", "г", "д", "е", "ё", "ж", "з", "и", "й", "к", "л", "м", "н", "о", "п", "р", "с", "т", "у", "ф", "х", "ц", "ч", "ш", "щ", "ъ", "ы", "ь", "э", "ю", "я"};
        String[] codes = new String[symbols.length];


        Random random = new Random();
        for (int i = 0; i < codes.length; i++) {
            codes[i] = String.valueOf(random.nextInt(codes.length * 100));
        }


        textField.getDocument().addDocumentListener(new DocumentListener() {
            @Override
            public void insertUpdate(DocumentEvent e) {

                updateFile();
            }

            @Override
            public void removeUpdate(DocumentEvent e) {

                updateFile();
            }

            @Override
            public void changedUpdate(DocumentEvent e) {


                updateFile();
            }

            private void updateFile() {

                String input = textField.getText();


                if (input.equals("99")) {
                    System.exit(0); // Выход из программы
                }


                String encryptedText = "";
                for (int i = 0; i < input.length(); i++) {
                    char symbol = input.charAt(i);
                    int index = -1;
                    for (int j = 0; j < symbols.length; j++) {
                        if (symbols[j].equals(String.valueOf(symbol))) {
                            index = j;
                            break;
                        }
                    }
                    if (index != -1) {
                        encryptedText += codes[index] + " ";
                    } else {
                    }
                }


                try (FileOutputStream fos = new FileOutputStream("C:\\Users\\Win10\\Desktop\\codes.txt", true)) {
                    PrintStream ps = new PrintStream(fos);
                    ps.println(encryptedText.trim());
                } catch (IOException ex) {
                    throw new RuntimeException(ex);
                }


                textField.setCaretPosition(0);
            }
        });


        jCheckBox.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                button.setEnabled(jCheckBox.isSelected());
            }
        });

        button.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                button.setEnabled(true);
                button2.setEnabled(true);
                System.out.println("Нажал на кнопку");
            }
        });

        button2.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                System.out.println("Нажал на кнопку шифрования");
                button3.setEnabled(true);
                textField.setEditable(true);
                textField.setText("");

            }
        });
      
        button3.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                System.out.println("Нажал на кнопку расшифрования, вводите цифровые коды");
                textField.setText("");
                textField.getDocument().addDocumentListener(new DocumentListener() {


                    @Override
                    public void insertUpdate(DocumentEvent e) {

                    }

                    @Override
                    public void removeUpdate(DocumentEvent e) {

                    }

                    @Override
                    public void changedUpdate(DocumentEvent e) {

                    }

                    private void updateFile() {
                        String input = textField.getText();
                       
                        if (input.equals("99")) {
                            System.exit(0); 
                        }
                        String decryptedText = ""; 
                        String[] codeParts = input.split("\\s+");
                        for (String codePart : codeParts) {
                            try {
                                int code = Integer.parseInt(codePart);
                                for (int j = 0; j < codes.length; j++) {
                                    if (codes[j].equals(String.valueOf(code))) { 
                                        decryptedText += symbols[j];

                                        break;
                                    }
                                }
                            } catch (NumberFormatException ignored) {

                            }
                            try (FileOutputStream fos = new FileOutputStream("C:\\Users\\Win10\\Desktop\\Новый текстовый документ (12).txt", true)) {
                                PrintStream ps = new PrintStream(fos);
                                ps.println(decryptedText.trim());
                            } catch (IOException ex) {
                                throw new RuntimeException(ex);
                            }
                        }

                    }
                });
            }
        });
    }
}
