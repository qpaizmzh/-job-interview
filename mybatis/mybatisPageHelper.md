# mybatisPageHelper结果集被合并的事情

- mybatisPageHelper不支持嵌套结果集的映射，这样会导致实际的分页数会比原来的要少，例如一个一对多关系的sql结果集查出来是n条，映射到java实例中就是一个实例嵌套n条数据；假设是一共分成K页，每页有L条实例数据，实际上每页的数目就是n/K个，而不是n/L个

- 这样的处理结果目前比较推荐的做法是在mybatis中的一对多映射的sql语句拆分成两个语句，并通过结果集进行关联映射，这样的话从一开始就会让该数据先映射到PageHelper，确定结果集的页数以及每页的数量，再另外调用嵌套结果集的语句，就不会影响PageHelper中设置的结果集，详情代码：

  ```xml
      <resultMap id="MyOrders" type="com.arch.vo.center.MyOrdersVO">
          <!--
            WARNING - @mbg.generated
          -->
          <id column="orderId" property="orderId"/>
          <result column="isComment" property="isComment"/>
          <result column="postAmount" property="postAmount"/>
          <result column="realPayAmount" property="realPayAmount"/>
          <result column="payMethod" property="payMethod"/>
          <result column="createdTime" property="createdTime"/>
          <result column="orderStatus" property="orderStatus"/>
          
  <!--        拆分的结果集用select属性指向另一个查询select标签的ID，
              column则是另一个语句关联的结果集的属性，用{}表示， 如果只有一个填属性名就可以,
  			这里的columnId和上面的id标签是对应的
              ofType填的就是另一个语句该映射的实体类的类型-->
          <collection property="subOrderItemList"
                      select="getOrderItemsByOrderId"
                      column="orderId"
                      ofType="com.arch.vo.center.MySubOrderItemVO">
              <result column="itemId" property="itemId"/>
              <result column="itemName" property="itemName"/>
              <result column="itemImg" property="itemImg"/>
  <!--            <result column="itemSpecId" property="itemSpecId"/>-->
              <result column="itemSpecName" property="itemSpecName"/>
              <result column="buyCounts" property="buyCounts"/>
              <result column="price" property="price"/>
          </collection>
      </resultMap>
  
  
      <select id="queryOrdersByUserId" resultMap="MyOrders">
          SELECT
          od.id as orderId,
          od.created_time as createdTime,
          od.pay_method as payMethod,
          od.real_pay_amount as realPayAmount,
          od.post_amount as postAmount,
          os.order_status as orderStatus,
          od.is_comment as isComment
          FROM orders od
          left join order_status os on od.id= os.order_id
          where od.user_id = #{userId}
          and od.is_delete=0
          <if test="orderStatus!=null and orderStatus!=''">
              and os.orderStatus=#{orderStatus}
          </if>
          order by
          od.updated_time ASC
      </select>
  
      <select id="getOrderItemsByOrderId" resultType="com.arch.vo.center.MySubOrderItemVO">
          SELECT
        oi.item_id as itemId,
        oi.item_name as itemName,
        oi.item_img as itemImg,
        oi.item_spec_name as itemSpecName,
        oi.buy_counts as buyCounts,
        oi.price as price
          FROM order_items oi
          where oi.order_id = #{orderId}
      </select>
  ```

  

  

