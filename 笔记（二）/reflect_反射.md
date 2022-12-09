

**###go reflect 注意事项：**###

**reflect.ValueOf(people1).Elem() // Elem()获取指针指向地址的封装**
	           **// 地址值必须使用Elem()才可以执行  不然panic: reflect: call of** 

**//使用反射进行修改值时，要求结构体中字段名首字母要大写**

  **// 结构体使用 tag`` 要获取都是使用reflect获取**



```
func main() {
	a:="Age"
	peo:=people{"miss",18}
	v:=reflect.ValueOf(peo)
	fmt.Println(v.NumField())
	fmt.Println(v.FieldByIndex([]int{0}))
	fmt.Println(v.FieldByName(a))
	
	people1 :=new(people)
	v =reflect.ValueOf(people1).Elem() // Elem()获取指针指向地址的封装
	           // 地址值必须使用Elem()才可以执行  不然panic: reflect: call of reflect.Value.FieldByName on ptr Value
	fmt.Println(v.FieldByName(a).CanSet())
	v.FieldByName(a).SetInt(20)//使用反射进行修改值时，要求结构体中字段名首字母要大写
	v.FieldByName("Name").SetString("an")
	fmt.Println(people1)
	
	
	/*
	获取tag
	 */
	t:=reflect.TypeOf(people{})
	fmt.Println(t.FieldByName("Name"))
	name,_:=t.FieldByName("Name")
	fmt.Println(name.Tag)
	fmt.Println(name.Tag.Get("json"))
	
}

type people struct {
	Name string   `json:"Name" `   // 结构体使用 tag`` 要获取都是使用reflect获取
	Age		int	  `json:"Age"`
}
```



