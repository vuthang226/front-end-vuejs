<template>
  <div class="material-content">
      <div class="title-content">
          <div class="title-content-left">
            <p class="title-text">Nguyên vật liệu</p>
          </div>
          <div class="title-content-right">
              <button class="btn-default"><span class="icon-feed-back"></span>Phản hồi</button>
          </div>
      </div>
       <div class="table-all">
            <div class="table-tool">
                <div class="btn-tool" @click="addMaterial()">
                    <div class="icon-add"></div>
                    <p class="custom-p">Thêm</p>
                </div>
                <div class="btn-tool" @click="cloneMaterial()">
                    <div class="icon-clone" ></div>
                    <p class="custom-p">Nhân bản</p>
                </div>
                <div class="btn-tool" @click="updateMaterial(materialFocus.materialId)">
                    <div class="icon-repair" ></div>
                    <p class="custom-p">Sửa</p>
                </div>
                <div class="btn-tool" @click="deleteMaterial()">
                    <div class="icon-delete" ></div>
                    <p class="custom-p">Xóa</p>
                </div>
                <div class="btn-tool" @click="refreshData()">
                    <div class="icon-refresh" ></div>
                    <p class="custom-p">Nạp</p>
                </div>   
            </div>
            <div class="table-content">
                <table id="table-Material" cellspacing="0" cellpadding="0" border="0" >
                        <thead>
                            <tr class="title-tb1">

                                <th  style="width:15%;">
                                    <div class="cell" @click="sortColumn(1)"><p>Mã nguyên vật liệu</p></div>
                                    <div class="table-search">
                                        
                                        <div class="btn-table-search">*</div>
                                        <input type="text" class="input-table-search" @change="filterSearch($event.target.value,1)"/>
                                    </div>
                                </th>
                                <th  style="width:22%">
                                    
                                    <div class="cell" @click="sortColumn(2)"><p>Tên nguyên vật liệu</p></div>
                                    <div class="table-search">
                                        <div class="btn-table-search">*</div>
                                        <input type="text" class="input-table-search" @change="filterSearch($event.target.value,2)"/>
                                    </div>
                                    </th>
                                <th style="width:20%">
                                    <div class="cell" @click="sortColumn(3)"><p>ĐVT tính</p></div>
                                    <div class="table-search">
                                        <div class="btn-table-search">*</div>
                                        <input type="text" class="input-table-search" @change="filterSearch($event.target.value,3)"/>
                                    </div>
                                    </th>
                                <th style="width:40%">
                                    <div class="cell" @click="sortColumn(4)"><p>Mô tả</p></div>
                                    <div class="table-search">
                                        <div class="btn-table-search">*</div>
                                        <input type="text" class="input-table-search" @change="filterSearch($event.target.value,4)"/>
                                    </div>
                                    </th>
                                
                                
                            </tr>
                        </thead>
                        <tbody class="tb1">
                            <tr 
                                v-for="material in Materials"
                                v-bind:key="material.materialId"
                                :class="{focusline:material.materialId == materialFocus.materialId}"
                                @click="
                                materialFocus = material;
                                "     
                                @dblclick="updateMaterial(material.materialId)">                  
                            <td>
                                <div class="cell" >{{ material.materialCode }}</div>
                                </td>
                                <td>
                                <div class="cell" >{{ formatEmpty(material.materialName) }}</div>
                                </td>
                                <td>
                                <div class="cell" >{{ formatEmpty(material.materialUnit) }}</div>
                                </td>
                                <td>
                                <div class="cell" >{{ formatEmpty(material.materialDescription) }}</div>
                                </td>
                                
                            </tr>
                
                        </tbody>
                </table>
            </div>

        </div> 


      <div class="paging-bar">
          <div class="paging-left">
                <button class="btn-square" @click="changePage(1)"><span class="icon-page-first"></span></button>
                <button class="btn-square" @click="nextPage(-1)"><span class="icon-page-prev"></span></button>
                
                <p class="paging-item">Trang</p>
                <input class="input-page" :value="pageNum" @input="changePage($event.target.value)" type="number"/>
                <p class="paging-item">trên 1</p>
                
                <button class="btn-square" @click="nextPage(1)"><span class="icon-page-next"></span></button>
                <button class="btn-square" @click="changePage(numPageMax)"><span class="icon-page-last"></span></button>
                <button class="btn-square"><span class="icon-page-load"></span></button>
          </div>
          <div class="paging-right">
              Hiển thị {{(this.pageNum -1)*30+1}}-{{this.ToEntities}} trên {{this.numOfMaterial}} kết quả 
              <div></div>
          </div>
      </div>
        <MaterialDialog
        :isDialogHide="isDialogHide"
        :ConvertUnits="ConvertUnitFilter"
        :material="material"
        :Stores="formatStores"
        :Units="formatUnits"
        :isAdd="isAdd"
        @btnCloseDialog="btnCloseDialog"
        @entityAdd="entityAdd"
        @btnNewRow="btnNewRow"
        @btnDeleteRow="btnDeleteRow"
        @saveData="saveData"
        @changeUnitMaterial="changeUnitMaterial"
        @changeMaterialCode="changeMaterialCode"
        @changeUnit="changeUnit"
        />
        <MaterialDialogAdd
        :isDialogAddHide="isDialogAddHide"
        :DialogAdd="DialogAdd[this.DialogAddType]"
        @btnCloseDialogAdd="btnCloseDialogAdd"
        @saveEntity="saveEntity"
        />
        <MaterialDialogPopUp
        :isDialogPopupHide="isDialogPopupHide"
        :msgPopUp="msgPopUp"
        :isDeleteCf="isDeleteCf"
        @btnCloseDialogPopup="btnCloseDialogPopup"
        @btnDeleteCf="btnDeleteCf"
        />
        <MaterialMessagePopUp
        :isMsgHide="isMsgHide"
        :msgUser="msgUser"
        @btnCloseMsg="btnCloseMsg"
        />
  </div>
</template>

<script>

import MaterialDialog from '@/components/Material/MaterialDialog.vue';
import MaterialDialogAdd from '@/components/Material/MaterialDialogAdd.vue';
import MaterialDialogPopUp from '@/components/Material/MaterialDialogPopUp.vue';
import MaterialMessagePopUp from '@/components/Material/MaterialMessagePopUp.vue'
import MaterialsAPI from '@/api/components/Materials/MaterialsAPI';
import StoresAPI from '@/api/components/Stores/StoresAPI';
import UnitsAPI from '@/api/components/Units/UnitsAPI';
export default {
    name:'MaterialContent',
    data(){
        return{
            /**Các biến phân trang và sắp sếp
             * Số trang 
             * số bản ghi 1 trang
             * cột sắp sếp
             * từ tìm kiếm
             * Cách sắp sếp
             */
            pageNum:1,
            numInPage:30,
            column:0,
            filter:"",
            type:0,   
            numOfMaterial:0,    
            /**Thanh menu rút gọn
             * vhthang 24/3/2020
             */
            MenuBarCus:[
                {class:"icon-menu-list", title:'Nguyên vật liệu'},
            ],
            /**Nguyên vật liệu 
             * Danh sách nguyên vật liệu
             * Danh sách chuyeerr đổi đơn vị
             * Danh sách kho hàng
             * Kho hàng
             * Danh sách đơn vị
             * Đơn vị
             * vhthang 17/3/2020
             */
            Materials:[], 
            material:{},
            materialFocus:{},
            ConvertUnits:[],
            Stores:[],
            store:{
                storeId:"b18323da-81a1-11eb-ab11-00fffc943f6f",storeName:""
            },
            Units:[],
            unit:{
                unitId:"b18323da-81a1-11eb-ab11-00fffc943f6f",unitName:""
            },
            /**Ẩn dialog chi tiết nguyên vật liệu*/
            isDialogHide:true,
            isDialogAddHide:true,
            isDialogPopupHide:true,
            isMsgHide:true,
            /**
             * Tiêu đề mục dialog add
             */
            DialogAdd:[
                {title:"Thêm đơn vị tính",name:"Đơn vị tính"},
                {title:"Thêm kho",name:"Tên kho"}
            ],
            /**
             * Nội dung đề mục dialog add
             * 0:ĐVT 1:Kho
             */
            DialogAddType:-1,
            /**
             * Mở dialog nhấn cất (Lưu - Thêm mới)
             * 0:Thêm mới  1:Update
             */
            isAdd:-1,
            /**Nội dung tin nhắn thông báo hiển thị dialog */
            msgPopUp:"Không có nội dung",
            /**Thông báo người dùng */
            msgUser:"",
            /**true:xoa false:Warning */
            isDeleteCf:true,
        }
    },  
    methods:{
        /**Hàm lấy danh sách nguyên vật liệu
         * vhthang 15/03/2021
         */
        async getMaterial(){
            const data = await MaterialsAPI.getDataByPageAndFilter(this.pageNum,this.numInPage,this.column,this.filter,this.type);
            this.Materials = data.data.data.materials;
            this.numOfMaterial = parseInt(data.data.data.numMaterials[0]);
        },

        /**Hàm lấy nguyên vật liệu và đơn vị chuyển đổi của nguyên vật liệu đó theo id
         * vhthang 15/03/2021
         */
        async getMaterialAndConvertById(id){   
            const res = await MaterialsAPI.getEntityById(id);
            this.material = res.data.data.material;
            this.ConvertUnits = this.formatConvertUnits(res.data.data.convertUnits);
        },

        /**Hàm lấy các kho chứa
         * vhthang 16/03/2021
         */
        async getStore(){
            const data = await StoresAPI.getAll();
            this.Stores = data.data.data;
        },

        /**Hàm lấy các kho chứa
         * vhthang 16/03/2021
         */
        async getUnit(){
            const data = await UnitsAPI.getAll();
            this.Units = data.data.data;
        },

        /**
         * Hàm lưu kho và đơn vị tính
         * sau khi lưu sẽ update lại kho và đơn vị tính <=============THIẾU
         * vhthang 19/03/2021
         */
        async saveEntity(data){
            if(data != null && data.trim() != ""){
                if(this.DialogAddType == 0){
                this.unit.unitName = data;
                const res = await UnitsAPI.insert(this.unit);
                this.callPopupMsg(res.data.message);
                
                this.getUnit();
                }else if (this.DialogAddType == 1){
                this.store.storeName = data;
                const res = await StoresAPI.insert(this.store);
                this.callPopupMsg(res.data.message);
                this.getStore();
                }
            }
            this.btnCloseDialogAdd();
        },

        /**Hàm xóa nvl
         * vhthang 19/03/2021
         */
        async btnDeleteCf(){
            const data = this.material;
            this.btnCloseDialogPopup(true);
            const res = await MaterialsAPI.delete(data);
            this.callPopupMsg(res.data.message);
            this.getMaterial();
        },

        /**Lưu dữ liệu thêm mới hoặc update
         * vhthang 24/03/2021
         */
        async saveData(type){
            if(this.isAdd == 0){
                const res = await MaterialsAPI.insert({
                material:this.material,
                convertUnits:this.formatConvertUnitsR(this.ConvertUnitsUpdate)
                });
                this.callPopupMsg(res.data.message);
                this.pageNum=1,
                this.column=0,
                this.filter="",
                this.type=0, 
                this.getMaterial();
            }else if(this.isAdd == 1){
                const res = await MaterialsAPI.update({
                material:this.material,
                convertUnits:this.formatConvertUnitsR(this.ConvertUnitsUpdate)
                });
                this.callPopupMsg(res.data.message);
                this.pageNum=1,
                this.column=0,
                this.filter="",
                this.type=0,        
                this.getMaterial();
            }
            this.material = {};
            this.ConvertUnits = [];
            this.isAdd=0;
            if(type == 0){this.btnCloseDialog();} //<= sai
        },

        
        refreshData(){
            this.pageNum=1,
            this.numInPage=30,
            this.column=0,
            this.filter="",
            this.type=0, 
            this.getMaterial();
        },

        /**Xử lý ẩn dialog
         * vhthang 16/03/2021
         */
        btnCloseDialog(){
            this.isAdd=-1;
            this.isDialogHide=true;
            this.material = {};
            this.ConvertUnits = [];        
        },

        /**Xử lý ẩn dialog add
         * vhthang 17/03/2021
         */
        btnCloseDialogAdd(){
            this.isDialogAddHide=true;
            this.DialogAddType=-1;
        },

        /**Xử lý ẩn dialog popup
         * vhthang 19/03/2021
         */
        btnCloseDialogPopup(iss){
            if(iss == true){
            this.material = {};
            }
            this.isDialogPopupHide=true;
        },

        /**Xử lý khi nhấn thêm một số thành phần phụ  
         * vhthang 17/03/2021
         */
        entityAdd(num){
            this.isDialogAddHide=false;
            this.DialogAddType=num;
        },

        /**
         * chỉnh định dạng convert
         * trả về arr ConvertUnits đã đc định dạng
         */
        formatConvertUnits(conv){
            for(let i = 0; i < conv.length;i++){
                if(conv[i].convertOperation == 1){conv[i].convertOperation = "*";}
                if(conv[i].convertOperation == 2){conv[i].convertOperation = "/";}
            }
            return conv;
            },
            formatConvertUnitsR(conv){
            for(let i = 0; i < conv.length;i++){
                if(conv[i].convertOperation == "*"){conv[i].convertOperation = 1;}
                if(conv[i].convertOperation == "/"){conv[i].convertOperation = 2;}
            }
            return conv;
        },

        /**Nhưng phương thức thực hiện trong trên màn hình chính */
        /** Xử lý khi thêm mới nguyên vật liệu
         * vhthang 19/03.2021
         */
        addMaterial(){
            this.isAdd=0;
            this.isDialogHide = false; 
        },

        /** Nhân bản dữ liệu (Lấy dữ liệu từ nvl đã chọn để thêm mới)
         * vhthang 19/03/2021
         */
        cloneMaterial(){
            this.isAdd=0;
            this.getMaterialAndConvertById(this.materialFocus.materialId);
            this.isDialogHide = false; 
        },

        /** Cập nhật dữ liệu lấy từ server chuyển chế độ update
         * vhthang 19/03/2021
         */
        updateMaterial(materialId){    //<= chỉnh lại ko tham số
            this.isAdd=1;
            this.getMaterialAndConvertById(materialId);
            this.isDialogHide = false; 
        },

        /**Xóa dữ liệu
         * vhthang 19/03/2021
         */
        deleteMaterial(){
            this.isDialogPopupHide=false;
            this.isDeleteCf=true;
            this.msgPopUp="Bạn có chắc chán muốn xóa Nguyên vật liệu <<" + this.materialFocus.materialCode + " - " + this.materialFocus.materialName + ">>  không?";
            this.material = this.materialFocus     //<= chỉnh lại ko cần đổi
        },

        /**Thêm dòng đơn vị chuyển đổi
         * vhthang 24/03/2021
         */
        btnNewRow(){
            this.ConvertUnits.push({
                convertId:"11452b0c-768e-5ff7-0d63-eeb1d8ed8cef",
                convertMaterialId:this.material.materialId,
                convertHandle:1,
                convertOperation:"*",
                convertUnitName:"",
                convertRatio:"1",
                convertDescription:""
            })
        },

        /**Xóa dòng đơn vị chuyển đổi
         * convertHandle 0:cập nhật 1:thêm mới 2:xóa 3:bỏ qua
         * vhthang 24/03/2021
         */
        btnDeleteRow(num){
            if(num == -1){
                if(this.ConvertUnits[this.ConvertUnits.length-1].convertHandle == 0){
                this.ConvertUnits[this.ConvertUnits.length-1].convertHandle = 2;
                }else if(this.ConvertUnits[this.ConvertUnits.length-1].convertHandle == 1){
                this.ConvertUnits.splice(this.ConvertUnits.length-1, 1);
                }
            }else{
                if(this.ConvertUnits[num].convertHandle == 0){
                this.ConvertUnits[num].convertHandle = 2;
                }else if(this.ConvertUnits[num].convertHandle == 1){
                this.ConvertUnits.splice(num,1);
                }
            }
        },

        /**
         * Gọi hàm hiện thông báo
         */
        callPopupMsg(msg){
            this.isMsgHide = false;
            this.msgUser = msg;
            setTimeout(() => { this.btnCloseMsg(); }, 5000)
        },

        /**Ẩn thông báo tin nhắn */
        btnCloseMsg(){
            this.isMsgHide = true;
            this.msgUser = "Thông báo ẩn";
        },

        /**
         * update mô tả của đơn vị chuyển đổi
         * vhthang24/03/2021
         */
        changeUnitMaterial(){
            for(let i = 0;i<this.ConvertUnits.length;i++){
                this.ConvertUnits[i].convertDescription ='1 '+this.ConvertUnits[i].convertUnitName+' = '+this.ConvertUnits[i].convertRatio+' '+this.ConvertUnits[i].convertOperation+' '+this.material.materialUnit
            }
        },

        /**
         * Kiểm tra trùng đơn vị tính
         * vhthang24/03/2021
         */
        changeUnit(num){
        // if(num > -1){
        //   if(this.ConvertUnitFilter[num].convertUnitName!= this.material.materialUnit){
        //     for(let i = 0 ;i<this.ConvertUnitFilter.length;i++)
        //     {
        //       if(this.ConvertUnitFilter[num].convertUnitName == this.ConvertUnitFilter[i].convertUnitName){
        //         this.ConvertUnitFilter[num].convertUnitName == null;
        //       }
        //     }
        //   }else this.ConvertUnitFilter[num].convertUnitName == null;
        // }
        
            for(let i = 0 ;i<this.UnitsExit.length;i++){
                if(this.ConvertUnitFilter[num].convertUnitName == this.UnitsExit[i]&&(num+1)!=(i)){
                this.msgPopUp="Đơn vị chuyển đổi không được trùng với đơn vị tính chính."
                this.isDeleteCf=false;
                this.isDialogPopupHide=false;
                }
            }
        },

        /**Chuyển trang đầu cuối 
         * vhthang24/03/2021
        */
        changePage(num){
                if(num < 1){
                this.pageNum = 1;
                }else if(num > this.numPageMax) this.pageNum = this.numPageMax;
                else this.pageNum = num;
                this.getMaterial();
        },

        /**Sắp xếp dữ liệu 
         * vhthang24/03/2021
        */
        sortColumn(sort){
        if(this.column != sort){
            this.column = sort;
            this.type = 0;
            }else {
            if(this.type == 0 )this.type = 1;
            else this.type = 0;
            }
        this.getMaterial();
        },

        /**
         * Luu ma code cao nhat
         * vhthang24/03/2021
         */
        changeMaterialCode(code){
        this.material.materialCode = code;
        },

        /**Timf kieems theo tuw 
         * vhthang24/03/2021
        */
        filterSearch(value,num){
        this.filter = value;
        this.column = num;
        console.log(value);
        this.getMaterial();
        },   
    },

    created(){
        /**Gọi danh sách dữ liệu khi load trang 
         * Danh sách nguyên vật liệu
         * Danh sách kho
         * Danh sách đơn vị
        */
        this.getMaterial();
        this.getStore();
        this.getUnit();
    },

    components:{       
        MaterialDialog,
        MaterialDialogAdd,
        MaterialDialogPopUp,
        MaterialMessagePopUp,
    },
    
    computed:{
        ToEntities:function(){
            if(((this.pageNum -1)*30+30) > this.numOfMaterial){
                return this.numOfMaterial;
            }else return ((this.pageNum -1)*30+30);
            
        },

        formatStores:function(){
            let stores = ['a'];
            stores = [];
            for(let i = 0; i < this.Stores.length;i++){
                stores[i] = this.Stores[i].storeName;
            }
            return stores;
        },

        formatUnits:function(){
            let units = ['a'];
            units = [];
            for(let i = 0; i < this.Units.length;i++){
                units[i] = this.Units[i].unitName;
            }
            return units;
        },
        
        ConvertUnitFilter:function(){
            return this.ConvertUnits.filter(function (convert) {
            return convert.convertHandle < 2
            })
        },

        ConvertUnitsUpdate:function () {
            return this.ConvertUnits.filter(function(convert){
                return (convert.convertUnitName );
            })
        },

        numPageMax:function(){
        return Math.ceil(this.numOfMaterial/this.numInPage);
        },

        UnitsExit:function(){
            const unitExit = [];
            if(this.material.materialUnit){
            unitExit.push(this.material.materialUnit);
            }
            for(let i=0;i<this.ConvertUnitFilter.length;i++){
                if(this.ConvertUnitFilter[i].convertUnitName != null||this.ConvertUnitFilter[i].convertUnitName != ""){
                unitExit.push(this.ConvertUnitFilter[i].convertUnitName);
                }
            }
            return unitExit;
        }
    

    },
    


}
</script>

<style>
.focusline{
    background-color: #d7e9f4 !important;
    cursor: pointer;
}
.material-content{
    width: 100%;
    height: 100%;
    background-color: #ffffff;
}
.material-content .paging-bar{
    height: 36px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0px 10px 0px 0px;
    z-index: 3;
    border: 1px solid #ccc;
    
}
.material-content .paging-bar .paging-left{
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.title-content{
    width: 100%;
    height: 40px;
    display:flex;
    justify-content: space-between;
    align-items: center;
}

.title-text{
    font-size: 22px;
}

.table-all{
    margin-top: 3px;
    height: calc(100% - 82px);
}
.table-content{
    width:100%;
    height: calc(100% - 32px);
    overflow: auto;
}
#table-Material{
    border-collapse:collapse;
    width: 100%;
    
    
    ;
}
/* cố định phần tiêu đề của bảng */
.table-content table th {
    z-index: 3;
    position: sticky;
    background-color: #ededed;
    /* background-color: #ededed; */
    top: 0px;
    left: 0px;
    
    /* border: 1px solid #E9E9E9; */
    border: 1px solid #cccccc;
    height: 49px;
    cursor: pointer;
    font-weight: normal;
    padding: 3px;
}

.table-search{
    display: flex;
}
.btn-table-search{
    width: 24px;
    height: 24px;
    border: 1px solid #d5d5d5;
    background-color: #fcfcfc;
    display: flex;
    justify-content: center;
    align-items: center;
}

/* Tạo css cho từng dòng phâng tử: tự động ẩn nếu phần chữ tràn */
.table-content table  td {
    border: 1px solid #cccccc;
    padding: 5px 10px 4px 10px !important;
    text-align: left;
    white-space: nowrap;
    box-sizing: border-box;
    text-overflow: ellipsis;
    word-break: break-all;
}

/* Tạo hover cho từng dòng dữ liệu */
.table-content tbody tr:nth-child(2n) {
    background-color: #fafafa;
}

.table-content tbody tr:hover {
    background-color: rgb(226, 239, 248);
    cursor: pointer;
}


.table-content td div {
    white-space: nowrap;
    box-sizing: border-box;
    overflow: hidden;
    text-overflow: ellipsis;
    word-break: break-all;
}



.icon-feed-back{
    background: url('../../assets/icon/IconSprite.png') no-repeat 0 -625px;
	width: 16px;
	height: 15px;
    margin-right:10px ;
}

.paging-item{
    width: 55px;
    margin-left: 3px ;
}
.table-all th .cell{
    display: flex;
    align-items: center;
    justify-content: center;
    height: 28px;
    width: 100%;
}
.table-all th .input-table-search{
    width: calc(100% - 25px);
    border: 1px solid #d5d5d5;
    outline: none;
    box-sizing: border-box;
}
.table-all th .input-table-search:focus{
    border: 1px solid #0072bc;
}

.table-tool{
    display: flex;
    height: 24px;
    background-image: -webkit-linear-gradient(top,#f9f9f9,#e3e4e6);
    width: calc(100% - 6px);
    color: black;
    align-items: center;
    background-color: #ededed!important;
    border-color:  #d9d9d9;
    border-style: solid;
    padding: 2px;
    border-width: 1px;
    border-bottom: none;
    
}
.btn-tool{
    display: flex;
    height: 16px;
    align-items: center;
    cursor: pointer; 
    padding: 2px 4px;
    justify-content: center;
}
.btn-tool:hover{
    background-color: #ffffff;
    border: 1px solid #0072bc;

}
.icon-add{
    background: url('../../assets/icon/IconSprite.png') no-repeat -1px -2648px;
	width: 14px;
	height: 16px;
    margin: 5px;
    
}
.icon-clone{
    background: url('../../assets/icon/IconSprite.png') no-repeat -1px -1888px;
	width: 14px;
	height: 15px;
    margin: 5px;
}
.icon-repair{
    background: url('../../assets/icon/IconSprite.png') no-repeat 0 -1872px;
	width: 15px;
	height: 15px;
    margin: 5px;
}
.icon-delete{
    background: url('../../assets/icon/IconSprite.png') no-repeat -2px -2683px;
	width: 12px;
	height: 11px;
    margin: 5px;
}
.icon-refresh{
    background: url('../../assets/icon/IconSprite.png') no-repeat 0 -2586px;
	width: 16px;
	height: 13px;
    margin: 5px;
}
.custom-p{
    margin-right:3px;
    font-size:12px
}

</style>