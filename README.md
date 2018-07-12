
[Source](https://threads-iiith.quora.com/Tutorial-on-Trie-and-example-problems "Permalink to Tutorial on Trie and example problems - Threads @ IIIT Hyderabad")

# Hướng dẫn về Trie và các ví dụ

Trong bài viết này tôi sẽ nói về Tries và khái niểm tổng quát được sử dụng trong thao tác nhỏ của vấn đề. Chúng ta sẽ thấy 2-3 vấn đề mà trie rất hữu ích

Đầu tiên chúng ta xem một trie là gì. Trie có thể lưu trữ thông tin về các khóa/số/chuỗi gọn gàng trong một cây.
Tries bao gồm các node, trong đó mỗi node lưu trữ một ký tự/bit. Chúng ta có thể chèn chuỗi/số mới cho phù hợp  
Dưới đây là ví dụ trie của chuỗi

![][1]

  
Nguồn: Wikipedia.

Nhưng chúng ta sẽ xử lý các con số ở đây, và đặc biệt trong các bit nhị phân. Chúng ta sẽ thấy khi chúng ta giải quyết vấn đề.

**Vấn đề 1**: Cho một mảng các số nguyên, chúng ta phải tìm hai phần tử mà khi XOR là lớn nhất.  
**Cách giải quyết:**  
Giả sử chúng ta có cấu trúc dữ liệu có thể đáp ứng hai kiểu truy vấn::  
1\. Chèn một số X  
2\. Cho Y, tìm XOR lớn nhất của Y với tất cả các số đã được thêm cho đến bây giờ.

Nếu chúng ta có cấu trúc dữ liệu này, Chúng ta sẽ thêm các số nguyên, và với truy vấn kiểu 2 chúng ta sẽ tìm thấy XOR lớn nhất.  
Trie là cấu trúc dữ liệu chúng ta sẽ sử dụng. Trước tiên, hãy xem cách chúng ta chèn các phần tử vào trie.

![][2]

  
Vì vậy, chúng ta theo đường dẫn của số chúng ta phải cần chèn, chúng ta không phải vẽ lại đường dẫn hiện có.

Chèn một khóa có độ dài N cần O(N) đó là log2(MAX) trong đó MAX là số lượng tối đa được chèn vào trong trie, vì có tối đa log2 (MAX) bit nhị phân trong một số.

Bằng cách này, chúng ta lưu trữ tất cả dữ liệu về tất cả các số được chèn vào trie cho đến bây giờ.

Bây giờ, với truy vấn kiểu 2:  

Giả sử số Y của chúng ta là b1,b2...bn, trong đó b1,b2.. là các bit nhị phân. Chúng ta bắt đầu từ b1. Bây giờ để XOR là lớn nhất, Chúng ta sẽ cố gắng tạo bit 1 quan trọng nhất sau khi XOR. Vì vậy, nếu b1 là 0, chúng ta sẽ cần 1 và ngược lại. Trong trie, chúng ta đi đến phía bit được yêu cầu. Nếu không có tùy chọn thuận lợi, chúng ta sẽ chuyển sang phía bên kia. Thực hiện điều này với i=1 đến n, chúng tôi sẽ nhận được XOR lớn nhất có thể.

![][3]

Truy vấn quá log2(MAX).

**Vấn đề 2**: Cho một mảng các số nguyên, tìm mảng con với XOR là lớn nhất.  
**Cách giải quyết:**  
Giả sử F (L, R) là XOR của mảng con từ L đến R. 
Ở đây chúng ta sử dụng tính chất F(L,R) = F(1,R) XOR F(1,L-1). Làm như thế nào?  
Giả sử mảng con của chúng ta với XOR lớn nhất kết thúc ở vị trí i. Bây giờ, chúng ta cần tối đa hóa F(L,i) ie. F(1,i) XOR F(1,L-1) trong đó L<=i. Giả sử, chúng ta chèn F(1,L-1) vào trie của chúng ta với mọi L<=i,thì đó chỉ là vấn đề 1
    
    
    ans=0
    pre=0
    Trie.insert(0)
    for i=1 to N:
        pre = pre XOR a[i]
        Trie.insert(pre)
        ans=max(ans, Trie.query(pre))
    print ans
    

Bạn có thể thử vấn đề này ở đây: [ACM-ICPC Live Archive][4]

**Vấn đề 3**: Với một mảng các số nguyên dương, bạn phải in ra số lượng các mảng con có XOR nhỏ hơn K.

**Cách giải quyết:**  
Điều này một lần nữa sử dụng các khái niệm mà chúng ta đã thấy cho đến bây giờ. Chúng ta sẽ làm như câu hỏi trước.  
Với mỗi index i=1 đến N, chúng ta có thể đếm số lượng mảng con kết thúc ở vị trí thứ i thỏa mãn điều kiện đã cho.

    
    
    ans=0
    p=0
    for i=1 to N:
        q=p XOR A[i]
        ans += Trie.query(q,k)
        Trie.insert(q)
        p=q
    

  
query(q,k) rả về số lượng các số nguyên đã tồn tại trong cấu trúc mà khi lấy XOR với q trả về một số nguyên nhỏ hơn k.
Chúng ta so sánh các bit tương ứng của q và k, bắt đầu từ các bit quan trọng nhất. Giả sử p và q là các bit tương ứng mà chúng ta đang xem xét.
Nếu q bằng 1, và p bằng 0, thì chúng ta thực hiện điều này:  

![][5]

Tương tự như vậy, chúng tôi có thể dễ dàng thực hiện 3 trường hợp khác. (q=1,p=1), (q=0,p=1) và (q=0,p=1).

Vì vậy, chúng ta cần phải thay đổi cấu trúc của chúng ta ở đây, chúng ta cũng giữ số lượng các node lá có thể truy cập từ node hiện tại nếu đi sang bên trái và tương tự cho phía bên phải. Bởi vì, nếu không, sự phức tạp sẽ tăng lên, nếu chúng ta lặp lại việc duyệt toàn bộ cây . Chúng ta có thể làm điều này trong khi chèn số vào cây rất dễ dàng

Vấn đề này đã được nêu trong CodeCraft'14. Bạn có thể thực hành tại: [SPOJ.com - Problem SUBXOR][6]

Bây giờ, chúng ta hãy nói về việc triển khai. 
Để thực hiện một trie trong C/CPP, chúng ta có thể giữ các node và các con trỏ trái và phải. Chúng ta có thể viết các hàm đệ quy. 

    
    
    insert(root, num, level):
        if level==-1: return root
        x=level'th bit of num
        if x==1:
            if root->right is NULL: create root->right
            else: insert(root->right,num,level-1)
        else:
            if root->left is NULL: create root->left
            else: insert(root->left,num,level-1)
    

  
Đối với các truy vấn,Chúng ta duyệt cây theo đệ quy.

Cập nhật:  
Một vấn đề khác khi sử dụng Trie(yay! :P).  
[Problem - B - Codeforces][7]

**Vấn đề con**: Cho một nhóm có _n_ chuỗi không trống. Trong trò chơi, hai người chơi cùng nhau xây dựng từ, ban đầu từ đó trống. Người chơi theo lần lượt. Trong lượt chơi của anh ta phải thêm một chữ cái vào cuối từ,từ kết quả phải là tiền tố của ít nhất một chuỗi trong nhóm. Một người chơi thua nếu anh ta không thể di chuyển.

Chúng ta cần phải tìm người chơi nào (thứ nhất hoặc thứ hai) có khả năng chiến thắng

Vì vậy, ý tưởng ở đây là một lần nữa để xây dựng một trie của tất cả các chuỗi. Tại sao? Bởi vì một trie lưu trữ thông tin về tất cả các tiền tố.
Bây giờ chúng ta sẽ thử đánh giá cho mỗi node nếu người chơi đầu tiên có khả năng thắng hoặc không.  Chúng ta có thể làm điều này một cách đệ quy. Với một node v, với mỗi node u sao cho u là con tức thời của v, nếu người chơi đầu tiên của node u có khả năng thua, thì sau đó người chơi đầu tiên của node v có khả năng chiến thắng.
Ví dụ, giả sử chúng ta có "abc", "abd", "acd".  
Trie của chúng ta sẽ như sau:

![][8]

  
Tất cả các node lá có khả năng chiến thắng.

[1]: https://qph.fs.quoracdn.net/main-qimg-aea28d9cd34aaf2d5783f4cd04e5abbd
[2]: https://qph.fs.quoracdn.net/main-qimg-388217a1992f1b2aac51e9917aa76d9c
[3]: https://qph.fs.quoracdn.net/main-qimg-e5d624e2cd693d713840a30ca9aaa461
[4]: https://icpcarchive.ecs.baylor.edu/index.php?Itemid=8&category=345&option=com_onlinejudge&page=show_problem&problem=2683
[5]: https://qph.fs.quoracdn.net/main-qimg-f24ea5ecf11805e7bcd82a48bb9cad25
[6]: http://www.spoj.com/problems/SUBXOR
[7]: http://codeforces.com/contest/455/problem/B
[8]: https://qph.fs.quoracdn.net/main-qimg-f81def67dffcc9e95306d65b27daa2f7-c

  
