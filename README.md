# Java-Project-Tracker
Console application for managing freelance projects. Features: file storage (txt), project statistics, and data persistence. Developed in Java.

import java.util.ArrayList;
import java.util.Scanner;
import java.io.*;


class Project {
    String name;
    int price;
    int days;
    int diff;

    Project(String name, int price, int days, int diff) {
        this.name = name;
        this.price = price;
        this.days = days;
        this.diff = diff;
    }
}

public class MainApp {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        ArrayList<Project> list = new ArrayList<>();


        try {
            BufferedReader reader = new BufferedReader(new FileReader("projects.txt"));
            String line;

            while ((line = reader.readLine()) != null) {
                String[] parts = line.split(",");

                if (parts.length == 4) {
                    String n = parts[0];
                    int p = Integer.parseInt(parts[1]);
                    int d = Integer.parseInt(parts[2]);
                    int diff = Integer.parseInt(parts[3]);

                    list.add(new Project(n, p, d, diff));
                }
            }

            reader.close();
            System.out.println("Проект в базі можете не переживати " + list.size());
        } catch (Exception e) {
            System.out.println("База порожня");
        }


        System.out.println("___Менеджер проектів___");
        while (true) {
            System.out.println("Виберіть дію (цифри) ");
            System.out.println("1 - додати проект ");
            System.out.println("2 - вигідніший ");
            System.out.println("3 - вихід ");
            System.out.println("4 - статистика ");
            System.out.println("5 - видалити ");

           int work = 0;
           try {
               work = sc.nextInt();
               sc.nextLine();

           } catch (Exception e) {
               System.out.println("Помилка введи цифру ");
               sc.nextLine();
               continue;
           }

            if (work == 1) {
                System.out.println("Назва проекту (Одним словом.)");
                String name = sc.next();
                System.out.println("Ціна проекту");
                int price = sc.nextInt();
                System.out.println("Дедлайн (дні) ");
                int days = sc.nextInt();
                System.out.println("Важкість від 1-10 ");
                int diff = sc.nextInt();

                list.add(new Project(name, price, days, diff));
                System.out.println("Проект додано в базу ");

            }

            if (work == 3) {
                try {
                    BufferedWriter writer = new BufferedWriter(new FileWriter("projects.txt"));
                    for (Project p : list) {
                        writer.write(p.name + "," + p.price + "," + p.days + "," + p.diff + "\n");
                    }
                    writer.close();
                    System.out.println(" Дані збережено у projects.txt.");
                } catch (IOException e) {
                    System.out.println("Помилка: " + e.getMessage());
                }
                break;
            }

            if (work == 4) {
                showStatistic(list);
            }

            if (work == 2) {
               showBestProject(list);
            }

            if (work == 5) {
                deleteProject(list, sc);
            }

        }
    }
    public static void showStatistic(ArrayList<Project> list) {

        if (list.isEmpty()) {
            System.out.println("Список пустий");
            return;
        }

        int totalSum = 0;
        int totalDays = 0;



        for (Project p : list ) {
            totalSum += p.price;
            totalDays += p.days;
        }

          int averagePerDay = totalSum / totalDays;

        System.out.println("Статистика");
        System.out.println("Кількість проектів: " + list.size());
        System.out.println("Загальний заробіток: " + totalSum + " грн");
        System.out.println("Днів на роботу: " + totalDays);



    }



    public static void showBestProject(ArrayList<Project> list) {
        if (list.isEmpty()) {
            System.out.println("Список пустий");
            return;
        } else {
            Project best = list.get(0);
            for (Project p : list) {
                if (p.price > best.price) {
                    best = p;
                }
            }
            System.out.println("Кращий проект по ціні");
            System.out.println("Назва: " + best.name);
            System.out.println("Ціна: " + best.price + " грн");
            System.out.println("Дедлайн: " + best.days + " дн.");
        }
    }


    public static void deleteProject(ArrayList<Project> list, Scanner sc) {
        if (list.isEmpty()) {
            System.out.println("База пуста ");
            return;
        }

        System.out.println("Номер проекту для видалення ");
        for (int i = 0; i < list.size(); i++ ) {
            System.out.println(i + " - " + list.get(i).name);
        }

        try {
            int index = sc.nextInt();
            sc.nextLine();


            if (index >= 0 && index < list.size()) {
                list.remove(index);
                System.out.println("Видалення");
            } else {
                System.out.println("Такого проекту немає в базі");
            }
        } catch (Exception e) {
            System.out.println("Помилка");
            sc.nextLine();
        }
    }
}
