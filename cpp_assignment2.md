# 18042030
# 任程威

## p301 9.11
```cpp
vector<int> vec;
vector<int> vec(10); 
vector<int> vec(10,1); 
vector<int> vec{1,2,3,4,5}; 
vector<int> vec(other_vec); 
vector<int> vec(other_vec.begin(), other_vec.end()); 
```
## p309 9.20
```
#include <iostream>
#include <deque>
#include <list>
using std::deque;
using std::list;
using std::cout;
using std::cin;
using std::endl;
int main()
{
    list<int> l{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    deque<int> odd, even;
for (auto i : l) (i & 0x1 ? odd : even).push_back(i);
for (auto i : odd) cout << i << " ";
    cout << endl;
for (auto i : even) cout << i << " ";
    cout << endl;
return 0;
}
```
## p315 9.29
```
vec.resize(100); // 在vector后面加75个
vec.resize(10); // 删去vector后面90个
```
## p324 9.43
```
#include <string>
using std::string;

#include <iostream>

void Replace(string& s, const string& oldVal, const string& newVal)
{
    for (auto beg = s.begin(); std::distance(beg, s.end()) >=
                               std::distance(oldVal.begin(), oldVal.end());) {
        if (string{beg, beg + oldVal.size()} == oldVal) {
            beg = s.erase(beg, beg + oldVal.size());
            beg = s.insert(beg, newVal.cbegin(), newVal.cend());
            std::advance(beg, newVal.size());
        }
        else
            ++beg;
    }
}

int main()
{
    {
        string str{"To drive straight thru is a foolish, tho courageous act."};
        Replace(str, "thru", "through");
        Replace(str, "tho", "though");
        std::cout << str << std::endl;
    }
    {
        string str{
            "To drive straight thruthru is a foolish, thotho courageous act."};
        Replace(str, "thru", "through");
        Replace(str, "tho", "though");
        std::cout << str << std::endl;
    } 
    {
        string str{"To drive straight thru is a foolish, tho courageous act."};
        Replace(str, "thru", "thruthru");
        Replace(str, "tho", "though");
        std::cout << str << std::endl;
    }
    {
        string str{"my world is a big world"};
        Replace(str, "world",
                "worlddddddddddddddddd");
        std::cout << str << std::endl;
    }
    return 0;
}
```
## p331 9.52
```
#include <stack>
using std::stack;

#include <string>
using std::string;

#include <iostream>
using std::cout;
using std::endl;

int main()
{
    auto& expr = "This is (***(*)((((*****o))))) and (*) over";
    auto repl = '#';
    auto seen = 0;

    stack<char> stk;

    for (auto c : expr) {
        stk.push(c);
        if (c == '(') ++seen;
        if (seen && c == ')') {
            while (stk.top() != '(') stk.pop();
            stk.pop();
            stk.push(repl);
            --seen;
        }
    }

    // Test
    string output;
    for (; !stk.empty(); stk.pop()) output.insert(output.begin(), stk.top());
    cout << output << endl;
}
```
## p339 10.3
```
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <numeric>

int main()
{
    std::vector<int> v = {1, 2, 3, 4};
    std::cout << std::accumulate(v.cbegin(), v.cend(), 0)
              << std::endl;
    return 0;
}
```
## p349 10.15
```
 int i = 42;
auto add = [i](int num){return i + num;};
```
## p365 10.34
```
#include <iostream>
#include <algorithm>
#include <list>
#include <vector>

int main()
{
    std::vector<int> v = {4, 5, 7, 9, 6, 3, 1, 0, 2, 3};

    //! 10.34
    for (auto iter = v.crbegin(); iter != v.crend(); ++iter)
        std::cout << *iter << " ";
}
```
## p370 10.42
```
#include <iostream>
#include <string>
#include <list>

using std::string;
using std::list;

void elimDups(list<string>& words)
{
    words.sort();
    words.unique();
}

int main()
{
    list<string> l = {"aa", "aa", "aa", "aa", "aasss", "aa"};
    elimDups(l);
    for (const auto& e : l) std::cout << e << " ";
    std::cout << std::endl;
}
```
## p381 11.12
```
#include <vector>
#include <utility>
#include <string>
#include <iostream>

int main()
{
    std::vector<std::pair<std::string, int>> vec;
    std::string str;
    int i;
    while (std::cin >> str >> i)
        vec.push_back(std::pair<std::string, int>(str, i));

    for (const auto& p : vec)
        std::cout << p.first << ":" << p.second << std::endl;
}
```
## p383 11.17
### 1.合法 2.不合法 3.合法 4.合法
## p396 11.38
```
#include <unordered_map>
#include <set>
#include <string>
#include <iostream>
#include <fstream>
#include <sstream>

using std::string;

void wordCounting()
{
    std::unordered_map<string, size_t> word_count;
    for (string word; std::cin >> word; ++word_count[word])
        ;
    for (const auto& w : word_count)
        std::cout << w.first << " occurs " << w.second
                  << (w.second > 1 ? "times" : "time") << std::endl;
}

void wordTransformation()
{
    std::ifstream ifs_map("../data/word_transformation.txt"),
        ifs_content("../data/given_to_transform.txt");
    if (!ifs_map || !ifs_content) {
        std::cerr << "can't find the documents." << std::endl;
        return;
    }

    std::unordered_map<string, string> trans_map;
    for (string key, value; ifs_map >> key && getline(ifs_map, value);)
        if (value.size() > 1)
            trans_map[key] =
                value.substr(1).substr(0, value.find_last_not_of(' '));

    for (string text, word; getline(ifs_content, text); std::cout << std::endl)
        for (std::istringstream iss(text); iss >> word;) {
            auto map_it = trans_map.find(word);
            std::cout << (map_it == trans_map.cend() ? word : map_it->second)
                      << " ";
        }
}

int main()
{
    wordTransformation();
}

```
## p446 13.12
### 3 accum item1 item2
## p452 13.18
```
#include "t13_18.h"

int Employee::s_increment = 0;

Employee::Employee()
{
    id_ = s_increment++;
}

Employee::Employee(const string& name)
{
    id_ = s_increment++;
    name_ = name;
}
```
t13_18.h
```
#ifndef CP5_t13_18_h
#define CP5_t13_18_h

#include <string>
using std::string;

class Employee {
public:
    Employee();
    Employee(const string& name);

    const int id() const { return id_; }

private:
    string name_;
    int id_;
    static int s_increment;
};

#endif
```
## p481 13.49
```
#ifndef CP5_STRING_H__
#define CP5_STRING_H__

#include <memory>

#ifndef _MSC_VER
#define NOEXCEPT noexcept
#else
#define NOEXCEPT
#endif

class String {
public:
    String() : String("") {}
    String(const char*);
    String(const String&);
    String& operator=(const String&);
    String(String&&) NOEXCEPT;
    String& operator=(String&&) NOEXCEPT;
    ~String();

    const char* c_str() const { return elements; }
    size_t size() const { return end - elements; }
    size_t length() const { return end - elements - 1; }

private:
    std::pair<char*, char*> alloc_n_copy(const char*, const char*);
    void range_initializer(const char*, const char*);
    void free();

private:
    char* elements;
    char* end;
    std::allocator<char> alloc;
};

#endif
```
```
#include "ex13_49_String.h"
#include <algorithm>

std::pair<char*, char*> String::alloc_n_copy(const char* b, const char* e)
{
    auto str = alloc.allocate(e - b);
    return {str, std::uninitialized_copy(b, e, str)};
}

void String::range_initializer(const char* first, const char* last)
{
    auto newstr = alloc_n_copy(first, last);
    elements = newstr.first;
    end = newstr.second;
}

String::String(const char* s)
{
    char* sl = const_cast<char*>(s);
    while (*sl) ++sl;
    range_initializer(s, ++sl);
}

String::String(const String& rhs)
{
    range_initializer(rhs.elements, rhs.end);
}

void String::free()
{
    if (elements) {
        std::for_each(elements, end, [this](char& c) { alloc.destroy(&c); });
        alloc.deallocate(elements, end - elements);
    }
}

String::~String()
{
    free();
}

String& String::operator=(const String& rhs)
{
    auto newstr = alloc_n_copy(rhs.elements, rhs.end);
    free();
    elements = newstr.first;
    end = newstr.second;
    return *this;
}

String::String(String&& s) NOEXCEPT : elements(s.elements), end(s.end)
{
    s.elements = s.end = nullptr;
}

String& String::operator=(String&& rhs) NOEXCEPT
{
    if (this != &rhs) {
        free();
        elements = rhs.elements;
        end = rhs.end;
        rhs.elements = rhs.end = nullptr;
    }
    return *this;
}
```
## p485 13.58
```
#include <vector>
#include <iostream>
#include <algorithm>

using std::vector;
using std::sort;

class Foo {
public:
    Foo sorted()&&;
    Foo sorted() const&;

private:
    vector<int> data;
};

Foo Foo::sorted() &&
{
    sort(data.begin(), data.end());
    std::cout << "&&" << std::endl; // debug
    return *this;
}

Foo Foo::sorted() const &
{
    //    Foo ret(*this);
    //    sort(ret.data.begin(), ret.data.end());
    //    return ret;

    std::cout << "const &" << std::endl; // debug

    //    Foo ret(*this);
    //    ret.sorted();  
    //    return ret;

    return Foo(*this).sorted(); 
}

int main()
{
    Foo().sorted(); 
    Foo f;
    f.sorted(); 
}
```
## p493 14.3
### 1.都不是2.string3.vector4.string
## p500 14.20
```
class Sales_data {
    friend std::istream& operator>>(std::istream&, Sales_data&);   
    friend std::ostream& operator<<(std::ostream&, const Sales_data&); 
    friend Sales_data operator+(const Sales_data&,
                                const Sales_data&); 

public:
    Sales_data(const std::string& s, unsigned n, double p)
        : bookNo(s), units_sold(n), revenue(n * p)
    {
    }
    Sales_data() : Sales_data("", 0, 0.0f) {}
    Sales_data(const std::string& s) : Sales_data(s, 0, 0.0f) {}
    Sales_data(std::istream& is);

    Sales_data& operator+=(const Sales_data&);
    std::string isbn() const { return bookNo; }

private:
    inline double avg_price() const;

    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
```
## p519 14.38
```
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
#include <fstream>

class BoundTest {
public:
    BoundTest(std::size_t l = 0, std::size_t u = 0) : lower(l), upper(u) {}
    bool operator()(const std::string& s)
    {
        return lower <= s.length() && s.length() <= upper;
    }

private:
    std::size_t lower;
    std::size_t upper;
};

int main()
{
    std::ifstream fin("../data/storyDataFile.txt");

    std::size_t quantity9 = 0, quantity10 = 0;
    BoundTest test9(1, 9);
    BoundTest test10(1, 10);

    for (std::string word; fin >> word;) {
        if (test9(word)) ++quantity9;
        if (test10(word)) ++quantity10;
    }

    std::cout << quantity9 << ", " << quantity10 << std::endl;
}
```
## p522 14.52
```
ld=ld + si;有歧义
ld=si + ld;两个都可以
```
## p539 15.12
### 有必要，override意味着重载父类中的虚函数，final意味着禁止子类重载该虚函数。两个用法并不冲突。
## p542 15.16
```
class Limit_quote : public Disc_quote
{
public:
    Limit_quote() = default;
    Limit_quote(const std::string& b, double p, std::size_t max, double disc):
        Disc_quote(b, p, max, disc)  {   }

    double net_price(std::size_t n) const override
    { return n * price * (n < quantity ? 1 - discount : 1 ); }
};
```
## p562 15.30
```
#include <iostream>
#include <memory>
#include <set>
#include "Quote.h"

using namespace std;

class Basket
{
public:
    void add_item(const shared_ptr<Quote> &sales)
    {
        items.insert(sales);
    }
    double total_receipt (std::ostream&) const; 
private:
    static bool compare(const std::shared_ptr<Quote> &lhs, const std::shared_ptr<Quote> &rhs)
    {
        return lhs->isbn() < rhs->isbn();
    }
    std::multiset<std::shared_ptr<Quote>, decltype(compare)*> items{compare};
};

double Basket::total_receipt(std::ostream &os) const
{
    double sum = 0.0;

    for (auto iter = items.cbegin(); iter != items.cend(); iter=items.upper_bound(*iter))
    {
        sum += print_total (os, **iter, items.count(*iter));
    }
    os << "Total Sale: " << sum << endl;
    return  sum;
}
```
