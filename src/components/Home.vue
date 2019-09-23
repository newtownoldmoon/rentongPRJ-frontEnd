<template>
  <div id="home">
    <!-- 表单组件 -->
    <el-form ref="from" v-model="form" label-width="85px">
      <el-form-item label="物件名：">
        <el-input v-model="form.title"></el-input>
      </el-form-item>
      <el-form-item label="物件地址：">
        <el-cascader v-model="form.address" :options="cityOption"></el-cascader>
      </el-form-item>
      <el-form-item label="租金：">
        <el-input type="number" v-model.number="form.rent"></el-input>
      </el-form-item>
      <el-form-item label="备注：">
        <el-input type="textarea" v-model="form.note"></el-input>
      </el-form-item>
      <el-button v-on:click="add()">新增物件</el-button>
      <el-button v-on:click="search()">查询物件</el-button>
    </el-form>
    <hr />
    <!-- 表格组件 -->
    <el-table :data="info">
      <el-table-column prop="title" label="物件名"></el-table-column>
      <el-table-column prop="address" label="物件地址"></el-table-column>
      <el-table-column prop="rent" label="租金"></el-table-column>
      <el-table-column prop="note" label="备注"></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="mini" v-on:click="edit(scope.row)">编辑</el-button>
          <el-button type="text" size="mini" v-on:click="remove(scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- 嵌套表单的对话框，修改用 -->
    <el-dialog title="修改物件信息" :visible.sync="dialogFormVisible">
      <el-form ref="from" v-model="formD" label-width="85px">
        <el-form-item label="物件名：">
          <el-input v-model="formD.title"></el-input>
        </el-form-item>
        <el-form-item label="物件地址：">
          <el-cascader v-model="formD.address" :options="cityOption"></el-cascader>
        </el-form-item>
        <el-form-item label="租金：">
          <el-input type="number" v-model.number="formD.rent"></el-input>
        </el-form-item>
        <el-form-item label="备注：">
          <el-input type="textarea" v-model="formD.note"></el-input>
        </el-form-item>
        <el-button type="text" v-on:click="dialogFormVisible = false">关闭</el-button>
        <el-button type="text" v-on:click="editConfirm()">修改</el-button>
      </el-form>
    </el-dialog>
    <!-- 回到顶部 -->
    <!-- <el-backtop target=".page-component__scroll .el-scrollbar__wrap"></el-backtop> -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      url: "http://127.0.0.1:2333/api/",
      //表格初始数据
      info: null,
      //表单获取数据
      form: {
        title: "",
        address: [],
        rent: 0,
        note: ""
      },
      //嵌套表单用的数据，虽然跟提交表单一模一样，但是我不知道怎样才能在互不影响的前提下共享数据，所以干脆复制了一份，修改时注意两份都要修改。
      formD: {
        title: "",
        address: [],
        rent: 0,
        note: ""
      },
      //城市选择器设置
      cityOption: [
        {
          label: "京都府",
          value: "京都府",
          children: [
            {
              label: "京都市",
              value: "京都市",
              children: [
                {
                  label: "北区",
                  value: "北区"
                },
                {
                  label: "上京区",
                  value: "上京区"
                },
                {
                  label: "左京区",
                  value: "左京区"
                },
                {
                  label: "中京区",
                  value: "中京区"
                },
                {
                  label: "東山区",
                  value: "東山区"
                },
                {
                  label: "山科区",
                  value: "山科区"
                },
                {
                  label: "下京区",
                  value: "下京区"
                },
                {
                  label: "南区",
                  value: "南区"
                },
                {
                  label: "右京区",
                  value: "右京区"
                },
                {
                  label: "西京区",
                  value: "西京区"
                },
                {
                  label: "伏見区",
                  value: "伏見区"
                }
              ]
            },
            {
              label: "福知山市",
              value: "福知山市"
            },
            {
              label: "舞鶴市",
              value: "舞鶴市"
            },
            {
              label: "綾部市",
              value: "綾部市"
            },
            {
              label: "宇治市",
              value: "宇治市"
            },
            {
              label: "宮津市",
              value: "宮津市"
            },
            {
              label: "亀岡市",
              value: "亀岡市"
            },
            {
              label: "城陽市",
              value: "城陽市"
            },
            {
              label: "向日市",
              value: "向日市"
            },
            {
              label: "長岡京市",
              value: "長岡京市"
            },
            {
              label: "八幡市",
              value: "八幡市"
            },
            {
              label: "京田辺市",
              value: "京田辺市"
            },
            {
              label: "京丹後市",
              value: "京丹後市"
            },
            {
              label: "南丹市",
              value: "南丹市"
            },
            {
              label: "木津川市",
              value: "木津川市"
            },
            {
              label: "大山崎町",
              value: "大山崎町"
            },
            {
              label: "久御山町",
              value: "久御山町"
            },
            {
              label: "井手町",
              value: "井手町"
            },
            {
              label: "笠置町",
              value: "笠置町"
            },
            {
              label: "和束町",
              value: "和束町"
            },
            {
              label: "精華町",
              value: "精華町"
            },
            {
              label: "南山城村",
              value: "南山城村"
            },
            {
              label: "京丹波町",
              value: "京丹波町"
            },
            {
              label: "伊根町",
              value: "伊根町"
            },
            {
              label: "与謝野町",
              value: "与謝野町"
            }
          ]
        },
        {
          label: "大阪府",
          value: "大阪府",
          children: [
            {
              label: "大阪市",
              value: "大阪市"
            },
            {
              label: "堺市",
              value: "堺市"
            },
            {
              label: "豊能地域",
              value: "豊能地域"
            },
            {
              label: "三島地域",
              value: "三島地域"
            },
            {
              label: "北河内地域",
              value: "北河内地域"
            },
            {
              label: "中河内地域",
              value: "中河内地域"
            },
            {
              label: "泉北地域",
              value: "泉北地域"
            },
            {
              label: "泉南地域",
              value: "泉南地域"
            },
            {
              label: "南河内地域",
              value: "南河内地域"
            }
          ]
        }
      ],
      dialogFormVisible: false,
      dialogFormId: ""
    };
  },
  methods: {
    //增
    add() {
      this.axios.post(this.url, this.form);
      this.$alert("新增了一条物件信息");
    },
    //删
    remove(row) {
      this.axios.delete(this.url + row._id);
      this.$alert("删除了一条物件信息");
    },
    //改
    edit(row) {
      this.dialogFormVisible = true;
      this.dialogFormId = row._id;
      this.formD = row;
    },
    editConfirm() {
      this.axios.put(this.url + this.dialogFormId, this.formD);
      this.$alert("修改了一条物件信息");
      this.dialogFormVisible = false;
    },
    //查
    //検索機能は同僚と相談してから作りますので、一旦放置します。
    search() {}
  },
  mounted() {
    this.axios
      .get("http://127.0.0.1:2333/api")
      .then(res => (this.info = res.data));
  }
};
</script>


<style scoped>
</style>
