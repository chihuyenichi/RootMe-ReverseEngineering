# Challenge: pwn/ELF-x64-Dobule-free 

## Description: Cho source-code.c khai báo các hàm, struct, biến được sử dụng 

## Analysis: 
- Xuất hiện nhiều hàm `free()` trong code nên ta sẽ tập trung vào chúng, sử dụng các phương thức như UAF/ Double-free 
- Để ý struct của Human và Zombie tương đồng về size -> khi khai báo con trỏ chúng sẽ được coi là cùng 1 size-class
    - Khi free con trỏ của 1 trong 2 kiểu struct này thì chúng sẽ thuộc 1 tcache (nơi lưu giữ freed chunk đươi dạng danh sách liên kết đơn)

- Phân tích 2 hàm `free()` tác dụng lên struct Human:
    - Ở struct Human tồn tại hàm `suicide()` có lệnh `free()` nhưng không có phép con trỏ = NULL -> Ta có thẻ khai thác bằng cách dùng UAF/ Double-free
        ```c
            puts("You can't survive at this zombie wave. *PAM*");
            memset(human, 0, sizeof(struct Human));
            free(human);
        ```
        - Không chỉ vậy, hàm còn có memset dữ liệu về 0 -> Khiến thư viện glibc không nhận biết được lỗi Dobule-free
            ```c
            Phát hiện double-free trong tcache:
            // glibc _int_free() - tcache_put
            static __always_inline void tcache_put(mchunkptr chunk, size_t tc_idx) {
                tcache_entry *e = (tcache_entry *)chunk2mem(chunk);
                if (__glibc_unlikely(e->key == tcache_key)) {
                    tcache_entry *tmp;
                    LIFO_CHECK(tmp); chunk â abort()
                }

                e->key = tcache_key;     data
                e->next = tcache->entries[tc_idx];
                tcache->entries[tc_idx] = e;
                ++(tcache->counts[tc_idx]);
            }
            ```
    - Ngoải ra còn một hàm sử dụng hàm `free()` với con trỏ Human nằm trong hàm `eatBody()` của struct Zombie; Hàm này cũng tương tự khi có hàm `memset()` khiến glibc không phát hiện được lỗi Double-free 

    - Như vậy với 2 hàm trên, ta có thể khiến tcache sẽ có dạng `H -> H -> NULL` (ở đây là H là địa chỉ lưu trữ (của con trỏ Human khi ta tạo nó), và chúng đều có giá trị giống nhau, gọi là trỏ đến chính mình (Self-loop))

- Từ kết quả trên ta có thể tạo lần lượt con trỏ Zombie, con trỏ Human và khiến chúng trỏ cùng vào một địa chỉ. Human dược tạo sau nên các con trỏ hàm tại địa chỉ chung đó sẽ là các hàm của Human -> Từ đó ta có thể gọi hàm in ra flag - `prayChuckToGiveAMiracle()`, thông qua hàm `eatBody()` của Zombie (vì 2 hàm này có chung offset trong struct) 
