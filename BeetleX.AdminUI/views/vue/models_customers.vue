﻿<div>
    <table class="table">
        <thead>
            <tr>
                <th>CustomerID</th>
                <th>CompanyName</th>
                <th>ContactName</th>
                <th>ContactTitle</th>
                <th>Phone</th>
                <th>Fax</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="item in items">
                <td><a href="javascript:void(0)" @click="OnOpen(item)"> {{item.CustomerID}}</a></td>
                <td>{{item.CompanyName}}</td>
                <td>{{item.ContactName}}</td>
                <td>{{item.ContactTitle}}</td>
                <td>{{item.Phone}}</td>
                <td>{{item.Fax}}</td>
            </tr>
            <tr v-if="GetCustomers.data.index<pages">
                <td colspan="6" style="text-align:right;">
                    <a href="javascript:void(0)" @click="GetCustomers.get()">({{GetCustomers.data.index}}/{{pages}})更多...</a>
                </td>
            </tr>
        </tbody>
    </table>
</div>
<script>
    {
        data: function () {
            return {
                GetCustomers: new beetlexAction("/Customers", { index: 0 }, { pages: 0, items: [] }),
                pages: 0,
                items: []
            }
        },
        methods: {
            OnOpen: function (item) {
                this.$open('cust' + item.CustomerID, '客户:' + item.CompanyName, 'models_customerdetail', { id: item.CustomerID });
            }
        },
        mounted: function () {
            var _this = this;
            this.GetCustomers.requested = function (r) {
                _this.pages = r.pages;
                this.data.index++;
                r.items.forEach(function (v) {
                    _this.items.push(v);
                });
            };
            this.GetCustomers.get();
        }
    }
</script>