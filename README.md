# multi-select
        getSelRows = function() {
             var rows = [];

             for(var i = 0; i < selRows.length; i++) {
                rows.push(selRows[i]);
             }

             rows.sort(function(r1, r2) {
                return r1 - r2;
             });

             return rows;
          }

         clearSelRows = function() {
            for(var i = selRows.length - 1; i >= 0; i--) {
               setRowSelected(selRows[i], false);
            }

            selRows = [];
         }

         setRowSelected = function(ridx, select) {
            var trs = $("tr", _this.body);

            for(var i = 0; i < trs.length; i++) {
               var row = trs[i];
               var id = $(row).attr("id");
               var idx = id.substring(id.length - 2, id.length);

               if(!row.isEmpty && idx== ridx) {
                  if(select){
                     $(row).attr("selected", "selected");
                  }
                  else {
                     $(row).removeAttr("selected");
                  }
                  break;
               }
            }

            if(select) {
               if($.inArray(ridx, selRows) == -1) {
                  selRows.push(ridx);
                  selRows.sort(function(r1, r2) {
                     return r1 - r2;
                  });
               }
            }
            else if(!select && $.inArray(ridx, selRows) != -1) {
               selRows.splice($.inArray(ridx, selRows), 1);
            }

         }

         onPress = function(event) {
            var tr = $("#" + event.data.gridId + " #" + event.data.trId);
            var id = $(tr).attr("id");
            var rid = id.substring(id.length - 2, id.length);
            if(event.ctrlKey == null && event.shiftKey == null) {
               if(event.button == 0) {
                  clearSelRows();
               }
               else if(event.button == 2) {
                  if(selRows.length <= 1 || $.inArray(rid, selRows) < 0) {
                     clearSelRows();
                  }
               }
            }

               var lastPressRow = selRows.length > 0 ? selRows[0] : null;

               if(event.ctrlKey == null && event.shiftKey == null || lastPressRow == null) {
                  setRowSelected(rid, true);
                  return;
               }
               else if(event.ctrlKey) {
                  setRowSelected(rid, true);
               }
               else if(event.shiftKey) {
                  if(rid == lastPressRow) {
                     return;
                  }

                  clearSelRows();

                  if(lastPressRow < rid) {
                     for(var i = lastPressRow; i <= rid; i++) {
                        setRowSelected(i, true);
                     }
                  }
                  else if(lastPressRow > rid) {
                     for(var i = lastPressRow; i >= rid; i--) {
                        setRowSelected(i, true);
                     }
                  }
               }
            }
