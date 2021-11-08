#include <iostream>
#include <iomanip>
#include <string>

using namespace std;

class Semester {
    struct Calculate {
        int size;
        string id, name;
        double red;
        Calculate* next;
    };
public:
    Semester() {
        head = NULL;
    }
    bool menu(int x) {
        if (head == NULL)
          return false;

        Calculate* t = new Calculate;
        t = head;
        while (t != NULL) {
          if (t->size == x)
            return true;
          t = t->next;
        }
        return false;
    }

    void calCgpa() {
        string id,name;
        double red;

        cout << "\n-------------- New Student -----------------" << endl;
        cout << endl;
        cout << "Enter ID Student : ";
        cin >> id;
        cout << "Enter Student Name : ";
        cin>>name;
        cout << "Enter Student CGPA : ";
        cin >> red;
        cout << "-----------------------------------\n\n" << endl;
        if(red <= 4.00 && red >= 0){
          insertGPA(&id, &name, &red);
           return ;
        }else{
          cout<<"Your cgpa pointer must below than [4.00]";
        }
    }

    int roll = 1;

    void insertGPA(string* id, string* name, double* red) {
        //add new Calculate to linked list
        // Create new Calculate to Insert Record
        Calculate* t = new Calculate();
        t->size = roll;
        t->id = *id;
        t->name = *name;
        t->red = *red;
        t->next = NULL;

        // Insert at Begin
        if (head == NULL || (head->size >= t->size)) {
            t->next = head;
            head = t;
        }

        // Insert at middle or End
        else {
            Calculate* c = head;
            while (c->next != NULL && c->next->size < t->size) {
                c = c->next;
            }
            t->next = c->next;
            c->next = t;
        }
        cout << "Student [" << t->size << "] : " << t->id << "\t\t" << t->name << "\t\t" << t->red << fixed << setprecision(2) << "\t\t";
        cout << "\n[Record] Inserted "
            << "Successfully.\n";
        roll = roll + 1;
    }

    void deleteAll() {
        cout << "\n[Record] Deleting Process..\n";
        Calculate* find = NULL;
        while (head != NULL ){
            //find = head->next;
            free(head);
            head = head->next;
        }
    }

    void deleteSel( string id) {
        Calculate* temp = head;
        Calculate* prev = NULL;
        cout << "\n[Record] Deleting Process..\n";
        if (temp != NULL && temp->id == id)
        {
            head = temp->next;
            delete temp;
            return;
        }
        else
        {
            while (temp != NULL && temp->id != id)
            {
                prev = temp;
                temp = temp->next;
            }
            if (temp == NULL)
                return;

            prev->next = temp->next;

            delete temp;
        }
    }

    double avg(Calculate* cur) {
        int count = 0; 
        double sum = 0.0;
        double avgi = 0.0;
        cur = head; 
        while (cur != NULL) {
            count++;
            sum = sum + cur->red;
            cur = cur->next;
        }
        // calculate average
        avgi = sum / count;
        return avgi;
    }

    void min(Calculate* p) {
        string id = head->id;
        string name = head->name;
        double min = head->red;
        cout << "\nLowest student Record : \nID\t\tNAME\t\tCGPA\n";
        while (p != NULL)
        {
            if ( min > p->red ) {
                id = p->id;
                name = p->name;
                min = p->red;        
            }
            p = p->next;
        }
        cout <<  id << "\t\t" << name << "\t\t" << min << "\t\t";
    }

    void max(Calculate* q) {
        q= head;
        string id = head->id;
        string name = head->name;
        double max = head->red;
        cout << "\nHighest student Record : \nID\t\tNAME\t\tCGPA\n";
        while (q != NULL )
        {
            if ( max < q->red) { 
              id = q->id;
              name = q->name;
              max = q->red; 
            }
            q = q->next;
        }
        cout <<  id << "\t\t" << name << "\t\t" << max << "\t\t";
    }

    void probation(Calculate* p) {
        p = head;
        string id = head->id;
        string name = head->name;
        double prob = head->red;
        cout << "\n\nProbation student Record : \nROLL\t\tID\t\tNAME\t\tCGPA\n";
        while (p != NULL )
        {
          if(p->red <= 2.00){
            id = p->id;
            name = p->name;
            prob = p->red;
            cout <<p->size <<"\t\t"<< id << "\t\t" << name << "\t\t" << prob<< "\t\t" << endl; 
          }
            p = p->next;
        }
    }

    void deanList(Calculate* z) {
        z = head;
        string id = head->id;
        string name = head->name;
        double dean = head->red;
        cout << "\n\nDean-List student Record : \nROLL\t\tID\t\tNAME\t\tCGPA\n";
        while (z != NULL )
        {
          if(z->red >= 3.50){
            id = z->id;
            name = z->name;
            dean = z->red;
            cout << z->size << "\t\t" << id << "\t\t" << name << "\t\t" << dean<< "\t\t" << endl;
          }
          z = z->next;
        }
    }

    void display(){
      Calculate *p=head;
      cout<<"\n ------------List All Student-----------"<<endl<<"Total Average : "<<avg(p);
      cout << "\nROLL\t|\t ID \t|\t NAME \t|\t CGPA\n";
      // Until p is not NULL
      
      while (p != NULL) {
          cout << endl
              << p->size << "\t\t|\t"
              << p->id << "\t\t|\t"
              << p->name << "\t\t|\t"
              << p->red << "\t\t";
          p = p->next;
      }
    }

    void displayPass() {
        int x;
        Calculate* p = head;
        cout << "[1] display the details of individual record\n";
        cout << "[2] display list all CGPA\n";
        cout << "[3] display average CGPA\n";
        cout << "[4] display Highest student record CGPA\n";
        cout << "[5] display Lowest student record CGPA\n";
        cout << "[6] All student with CGPA <= 2.00 (probation)\n";
        cout << "[7] All student with CGPA >= 3.50 (dean list)\n";
        cout << "Your Selection : ";
        cin >> x;
        if(p==NULL) {
          display();
          cout << "No Record Available\n\n";
          return;
        }
        else{
          if( x == 1) {
            string temp;
            cout << "Enter Student ID: "; cin >> temp;
            while(p!=NULL)
            {
              if(p->id == temp)
              {
                
                cout <<"\n\nRecord Student :"
                     << "\nName     : " << p->name << endl;
                cout << "Student ID : " << p->id << endl;
                cout << "CGPA       : " << p->red << endl << endl;
              }
              p = p->next;
            }
          }
          else if (x == 2) 
            display();
          else if (x == 3) 
            cout << endl << "Average Student CGPA : "<<avg(p);
          else if (x == 4)
            max(p);
          else if (x == 5) 
            min(p);
          else if (x == 6) 
            probation(p);
          else if (x == 7) 
            deanList(p);
      }
    }
private:
    Calculate* head;
};

int main() {
    //create class object and variables
    Semester list;
    int x = 0, choice;
    string reason;
    do {
        cout << "\n\n\t\tWelcome to Student Record "
            "\n\n\tPress\n\t1 to "
            "Register new Student Record\n\t2 to display all application "
            "\n\t3 to delete a recorded "
            "Record\n\t5 to Exit\n";
        cout << "\nEnter your Choice : ";
        choice = list.menu(x);
        cin >> choice;

        switch (choice) {
          case 1: list.calCgpa(); break;
          case 2: list.displayPass(); break;
          case 3: 
            string id;
            cout<<"Enter student ID\n";cin>>id;
            list.deleteSel(id); 
            break;
        }
    } while (choice != 5);
    list.deleteAll();
    cout<<"\n[Record] All deleted\n";
    list.display();
    cout<<endl;
}