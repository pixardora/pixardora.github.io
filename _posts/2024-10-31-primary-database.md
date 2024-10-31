---
layout: post
title:  "Primary Source Organizer"
date:   2024-10-31
tags: [techniques]
---
As the research goes on, I have collected way too many primary sources. They are in diverse materiality and have different connection with the research. I have long wanted to build a small database to a) collect all the primary sources;b)write brief annotations;c)categorize them for later use. It should be searchable as well. I ask Claude to write it for me. It actually looks not bad.


```
import sqlite3
import datetime
from tabulate import tabulate  # You'll need to install this with: pip install tabulate

class ResearchDatabase:
    def __init__(self):
        self.conn = sqlite3.connect('research.db')
        self.create_table()
        
    def create_table(self):
        cursor = self.conn.cursor()
        cursor.execute('''
        CREATE TABLE IF NOT EXISTS research_entries (
            id INTEGER PRIMARY KEY,
            source TEXT,
            analysis TEXT,
            category TEXT,
            date_added TIMESTAMP
        )
        ''')
        self.conn.commit()
        
    def add_entry(self):
        print("\nEnter new research entry:")
        source = input("Enter primary source text: ")
        analysis = input("Enter your analysis: ")
        category = input("Enter category: ")
        date_added = datetime.datetime.now()
        
        cursor = self.conn.cursor()
        cursor.execute('''
        INSERT INTO research_entries (source, analysis, category, date_added)
        VALUES (?, ?, ?, ?)
        ''', (source, analysis, category, date_added))
        self.conn.commit()
        print("Entry added successfully!")
    
    def view_entries(self, mode='all', filter_value=None):
        cursor = self.conn.cursor()
        
        if mode == 'all':
            cursor.execute('SELECT id, category, source, analysis, date_added FROM research_entries')
        elif mode == 'category':
            cursor.execute('SELECT id, category, source, analysis, date_added FROM research_entries WHERE category = ?', (filter_value,))
        elif mode == 'search':
            search_term = f'%{filter_value}%'
            cursor.execute('''
            SELECT id, category, source, analysis, date_added 
            FROM research_entries 
            WHERE source LIKE ? OR analysis LIKE ? OR category LIKE ?
            ''', (search_term, search_term, search_term))
            
        entries = cursor.fetchall()
        
        if not entries:
            print("\nNo entries found.")
            return
            
        headers = ['ID', 'Category', 'Source', 'Analysis', 'Date Added']
        # Format entries for display, truncating long text
        formatted_entries = []
        for entry in entries:
            formatted_entry = list(entry)
            formatted_entry[2] = formatted_entry[2][:50] + '...' if len(formatted_entry[2]) > 50 else formatted_entry[2]
            formatted_entry[3] = formatted_entry[3][:50] + '...' if len(formatted_entry[3]) > 50 else formatted_entry[3]
            formatted_entries.append(formatted_entry)
            
        print(tabulate(formatted_entries, headers=headers, tablefmt='grid'))
    
    def view_full_entry(self, entry_id):
        cursor = self.conn.cursor()
        cursor.execute('SELECT * FROM research_entries WHERE id = ?', (entry_id,))
        entry = cursor.fetchone()
        
        if entry:
            print(f"\n=== Entry {entry[0]} ===")
            print(f"Category: {entry[3]}")
            print(f"Date Added: {entry[4]}")
            print("\nSource:")
            print(entry[1])
            print("\nAnalysis:")
            print(entry[2])
        else:
            print("Entry not found.")
    
    def get_categories(self):
        cursor = self.conn.cursor()
        cursor.execute('SELECT DISTINCT category FROM research_entries')
        return [row[0] for row in cursor.fetchall()]

def main():
    db = ResearchDatabase()
    
    while True:
        print("\n=== Research Database Manager ===")
        print("1. Add new entry")
        print("2. View all entries (summary)")
        print("3. View by category")
        print("4. Search entries")
        print("5. View full entry by ID")
        print("6. Exit")
        
        choice = input("Enter your choice (1-6): ")
        
        if choice == '1':
            db.add_entry()
        elif choice == '2':
            db.view_entries()
        elif choice == '3':
            categories = db.get_categories()
            print("\nAvailable categories:")
            for i, category in enumerate(categories, 1):
                print(f"{i}. {category}")
            if categories:
                cat_choice = input("\nSelect category number: ")
                try:
                    selected_category = categories[int(cat_choice)-1]
                    db.view_entries('category', selected_category)
                except (ValueError, IndexError):
                    print("Invalid selection.")
            else:
                print("No categories found.")
        elif choice == '4':
            search_term = input("Enter search term: ")
            db.view_entries('search', search_term)
        elif choice == '5':
            entry_id = input("Enter entry ID: ")
            try:
                db.view_full_entry(int(entry_id))
            except ValueError:
                print("Invalid ID.")
        elif choice == '6':
            print("Goodbye!")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()

```
Then it looks like this: pretty cool:

=== Research Database Manager ===
1. Add new entry
2. View all entries (summary)
3. View by category
4. Search entries
5. View full entry by ID
6. Exit
Enter your choice (1-6): 1

Enter new research entry:
Enter primary source text: 1751皇清職貢圖
Enter your analysis: earliest record of this occupation
Enter category: official 
Entry added successfully!

=== Research Database Manager ===
1. Add new entry
2. View all entries (summary)
3. View by category
4. Search entries
5. View full entry by ID
6. Exit
Enter your choice (1-6): 1

Enter new research entry:
Enter primary source text: 難忘上學路
Enter your analysis: the landscape of the river valley, the mountains, and the state's purchasing station
Enter category: individual 
Entry added successfully!

=== Research Database Manager ===
1. Add new entry
2. View all entries (summary)
3. View by category
4. Search entries
5. View full entry by ID
6. Exit
Enter your choice (1-6): 4
Enter search term: river valley
+------+------------+------------+-------------------------------------------------------+----------------------------+
|   ID | Category   | Source     | Analysis                                              | Date Added                 |
+======+============+============+=======================================================+============================+
|    2 | individual | 難忘上學路 | the landscape of the river valley, the mountains, ... | 2024-10-31 01:39:24.758977 |
+------+------------+------------+-------------------------------------------------------+----------------------------+

=== Research Database Manager ===
1. Add new entry
2. View all entries (summary)
3. View by category
4. Search entries
5. View full entry by ID
6. Exit
Enter your choice (1-6): 2
+------+------------+----------------+-------------------------------------------------------+----------------------------+
|   ID | Category   | Source         | Analysis                                              | Date Added                 |
+======+============+================+=======================================================+============================+
|    1 | official   | 1751皇清職貢圖 | earliest record of this occupation                    | 2024-10-31 01:38:19.851979 |
+------+------------+----------------+-------------------------------------------------------+----------------------------+
|    2 | individual | 難忘上學路     | the landscape of the river valley, the mountains, ... | 2024-10-31 01:39:24.758977 |
+------+------------+----------------+-------------------------------------------------------+----------------------------+

=== Research Database Manager ===
1. Add new entry
2. View all entries (summary)
3. View by category
4. Search entries
5. View full entry by ID
6. Exit
Enter your choice (1-6): 
