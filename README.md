# vue 中 for 循环出的加验证
解决方法如下：
1.首先是循环表单项：
<el-form ref="form" :model="form" :rules="rules" label-width="120px">
    <el-col :span="20">
      <el-row :gutter="10" v-for="(item,index) in form.productGroup" :key="index">
       <el-col :span="6">
        <el-form-item label="商品数量:">
         <el-input v-model="item.num" type="number" size="small" style="width:80px;"></el-input>
        </el-form-item>
       </el-col>
       <el-col :span="6">
        <el-form-item label="优惠价格:">
          <el-input v-model="item.price" type="number" size="small" style="width:80px;"></el-input>
        </el-form-item>
       </el-col>
      </el-row>
    </el-col>
   </el-row>
 </el-form>
先是添加rules规则，这个和正常添加规则一样：

productGroupRules: {
 productGroupNum: [{required: true, message: '请填写商品数量', trigger: 'blur'}],
 
 productGroupPrice: [{required: true, message: '请填写优惠价格', trigger: 'blur'}]
}

然后给表单项添加验证，以商品数量为例：

<el-form-item label="商品数量:" :prop="'productGroup.'+index+'.num'" :rules="productGroupRules.productGroupNum">
 
 <el-input v-model="item.num" type="number" size="small" style="width:80px;"></el-input>
 
</el-form-item>

注意这里:rules是每个表单项都要添加，有多个的话用productGroupRules.productGroupNum这样的形式区分，对应上面productGroupRules里的内容。

另外主要就是:prop了，注意正常验证表单项是prop，而这里是:prop。 :prop="'productGroup.'+index+'.num'"是拼接的形式，前面是v-for绑定的数组，中间是数组索引index，最后是表单项绑定的v-model的名称，然后用点.把它们连接起来。这三项都必须保证正确，错一个都无法验证。
