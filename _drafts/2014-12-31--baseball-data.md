---
layout: post
title: Computing baseball eliminations
category: articles
---

Download http://www.retrosheet.org/gamelogs/gl1974.zip

```
csvcut -c 1,4,7,10,11 GL1974.TXT > gamelog-2014.csv

(echo Date,Team1,Team2,Score1,Score2; csvcut -c 1,4,7,10,11 GL1974.TXT)  >| games-1974.csv
(echo Date,Team1,Team2,Score1,Score2; cut -d,  -f1,4,7,10,11 GL1974.TXT)  >| games-1974.csv
(echo Date,Team1,Team2,Score1,Score2; cut -d,  -f1,4,7,10,11 GL1974.TXT | sed -e 's/^"\(....\)\(..\)\(..\)\(.*\)/"\1\/\2\/\3\4/') >| games-1974.csv
```

## Parsing historical baseball data

Ok, I couldn't hold off. I first tried to write some clever data munching in Mathematica (my go to tool lately), but I found it unnerving and whipped up some simple tally tool in java: 
    
        import java.util.HashMap;  
        import java.util.HashSet;  
        import java.util.LinkedList;  
        import java.util.List;  
        import java.util.Map;  
        import java.util.Set;  
    
        /**  
         * User: manuel  
         * Date: 12/31/14  
         */  
        public class ParseBaseball {  
            static class TeamStanding {  
                String             name;  
                int wins;  
                int losses;  
                int total;  
                int N;  
    
                int games[];  
                int totalGames[];  
    
                TeamStanding(String name, int N) {  
                    this.N = N;  
                    this.name = name;  
                    this.games = new int[N];  
                    this.totalGames = new int[N];  
                }  
    
                public String toString() {  
                    StringBuilder sb = new StringBuilder();  
    
                    sb.append(name + " " + wins + " " + losses + " " + (total - wins - losses));  
                    for (int i = 0; i < N; i++) {  
                        sb.append(" " + (totalGames[i] - games[i]));  
                    }  
    
                    return sb.toString();  
                }  
            }  
    
            static class GameEntry {  
                public Date   date;  
                public String team1;  
                public String team2;  
                public int score1;  
                public int score2;  
    
                GameEntry() {  
                }  
    
                public String toString() {  
                    return date.toString() + " - " + team1 + " vs. " + team2 + " : " + score1 + "-" + score2;  
                }  
            }  
    
            static String teamNames[];  
            static int N;  
            static Map<String,Integer> teamIds;  
    
            static int id(String team) {  
                return teamIds.get(team);  
            }  
    
            static Date parseDate(String s) {  
                final int day = Integer.parseInt(s.substring(6, 8));  
                final int month = Integer.parseInt(s.substring(4, 6));  
                final int year = Integer.parseInt(s.substring(0, 4));  
                return new Date(month, day, year);  
            }  
    
            /*  
             * data from: http://www.retrosheet.org/gamelogs/index.html  
             *  
             *  
             * cat GL1996.TXT | grep -v -e NL | cut -d,  -f1,4,7,10,11  | sed -e 's/"//g' >| gl1996.csv  
             *  
             * java -classpath .:stdlib.jar:algs4.jar ParseBaseball ~/Downloads/1974eve/gl1996.csv  
             */  
            public static void main(String[] args) {  
                In in = new In(args[0]);  
    
                List<GameEntry> entries = new LinkedList<GameEntry>();  
                Set<String> teams = new HashSet<String>();  
    
                while (in.hasNextLine()) {  
                    String input = in.readLine();  
                    String fields[] = input.split(",");  
    
                    GameEntry ge = new GameEntry();  
                    ge.date = parseDate(fields[0]);  
                    ge.team1 = fields[1];  
                    ge.team2 = fields[2];  
                    ge.score1 = Integer.parseInt(fields[3]);  
                    ge.score2 = Integer.parseInt(fields[4]);  
    
                    teams.add(ge.team1);  
                    teams.add(ge.team2);  
    
        //            StdOut.println(ge);  
                    entries.add(ge);  
                }  
    
                teamNames = teams.toArray(new String[0]);  
                N = teamNames.length;  
                teamIds = new HashMap<String,Integer>();  
    
                for (int i = 0; i < teamNames.length; i++) {  
                    teamIds.put(teamNames[i], i);  
                }  
    
                TeamStanding standings[] = new TeamStanding[N];  
                for (int i = 0; i < N; i++) {  
                    standings[i] = new TeamStanding(teamNames[i], N);  
                }  
    
                Date date = parseDate(args[1]);  
                for (GameEntry ge : entries) {  
                    final int id1 = id(ge.team1);  
                    TeamStanding ts1 = standings[id1];  
                    final int id2 = id(ge.team2);  
                    TeamStanding ts2 = standings[id2];  
    
                    ts1.totalGames[id2]++;  
                    ts1.total++;  
                    ts2.totalGames[id1]++;  
                    ts2.total++;  
    
                    if (ge.date.compareTo(date) >= 0) {  
                        continue;  
                    }  
    
                    ts1.games[id2]++;  
                    ts2.games[id1]++;  
    
                    if (ge.score1 > ge.score2) {  
                        ts1.wins++;  
                        ts2.losses++;  
                    } else {  
                        ts2.wins++;  
                        ts1.losses++;  
                    }  
                }  
    
        //        for (int i = 0; i < N; i++) {  
        //            StdOut.println(teamNames[i]);  
        //        }  
    
                StdOut.println(N);  
                for (int i = 0; i < N; i++) {  
    
                    StdOut.println(standings[i]);  
                }  
            }  
        }  

This will tally up the game data for a game downloaded from: [](http://www.retrosheet.org/gamelogs/index.html)[http://www.retrosheet.org/gamelogs/index.html](http://www.retrosheet.org/gamelogs/index.html) up to (including) a certain date. Note that the data needs to be massaged a bit before passing it into the java program:

  
    cat GL1996.TXT | grep -v -e OAK -e CHA -e CAL -e KCA -e CLE -e MIL -e MIN -e SEA -e TEX -e NL | cut -d,  -f1,4,7,10,11  | sed -e 's/"//g' >| gl1996.csv

  
For example: 
    
    m (master) src$ java -classpath .:stdlib.jar:algs4.jar ParseBaseball ~/Downloads/1974eve/gl1996.csv 19960831
    14
    OAK 65 72 25 0 0 2 0 3 3 3 2 0 0 3 7 2 0
    CHA 72 64 26 0 0 0 3 2 0 3 6 3 0 7 0 0 2
    CAL 62 73 26 2 0 0 3 2 2 4 0 0 0 3 3 7 0
    BAL 72 63 28 0 3 3 0 0 3 0 2 7 1 0 2 0 7
    KCA 61 75 25 3 2 2 0 0 0 7 0 2 0 3 3 0 3
    NYA 75 59 28 3 0 2 3 0 0 0 8 3 2 0 0 0 7
    CLE 80 54 27 3 3 4 0 7 0 0 0 0 3 3 2 2 0
    BOS 69 66 27 2 6 0 2 0 8 0 0 3 3 0 3 0 0
    DET 49 86 27 0 3 0 7 2 3 0 3 0 6 0 0 0 3
    MIL 65 71 26 0 0 0 1 0 2 3 3 6 0 2 0 7 2
    MIN 67 68 27 3 7 3 0 3 0 3 0 0 2 0 3 3 0
    SEA 70 64 27 7 0 3 2 3 0 2 3 0 0 3 0 4 0
    TEX 76 59 28 2 0 7 0 0 0 2 0 0 7 3 4 0 3
    TOR 63 72 27 0 2 0 7 3 7 0 0 3 2 0 0 3 0

This shows the data in the programming assignment example (I think the number of teams was reduced by merging them all into "Boston", but other than that the data seems to be the same). 

Letting my assignment run on it, we get 
    
    DET is eliminated by the subset R = { CLE }

More interesting is that DET is actually eliminated on august the 20th, but a quite crazy combination of teams:

  
    m (master) src$ java -classpath .:stdlib.jar:algs4.jar ParseBaseball ~/Downloads/1974eve/gl1996.csv 19960820 >| teams6.txt
    m (master) src$ java -classpath .:stdlib.jar:algs4.jar BaseballElimination teams6.txt
    OAK is not eliminated
    CHA is not eliminated
    CAL is not eliminated
    BAL is not eliminated
    KCA is not eliminated
    NYA is not eliminated
    CLE is not eliminated
    BOS is not eliminated
    DET is eliminated by the subset R = { OAK CHA CAL BAL KCA NYA CLE BOS MIL MIN SEA TEX TOR }
    MIL is not eliminated
    MIN is not eliminated
    SEA is not eliminated
    TEX is not eliminated
    TOR is not eliminated