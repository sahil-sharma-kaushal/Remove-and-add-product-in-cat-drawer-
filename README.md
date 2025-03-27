========# Remove-and-add-product-in-cat-drawer-==============
$(document).on('click', '.quantity.cart-quantity .quantity__button', function() {
  event.preventDefault()
    var vari_ids = [];
    var variant_id = $(this).closest('.cart-item').data('product-id');
    var qty = $(this).closest('.quantity').find('.quantity__input').val();
    var productId = $(this).parents('.cart-item ').find('.cart-item__details').find('.product-option:has(dt:contains("product_id")) dd').text().trim();
  // alert('productId' +  productId);
    {% for product in collections['all'].products %}
        if (productId == {{ product.id }}) {
            {% for variant in product.variants %}
                vari_ids.push('{{ variant.id }}');
            {% endfor %}
          // alert('reahed');
            if (qty == 1) {
              // alert('if');
               setTimeout(function(){
                $.ajax({
                    type: "POST",
                    url: "/cart/change.js",
                    data: {
                        id: variant_id,
                        quantity: 0  // Set the quantity to 0 to remove the item
                    },
                    success: function(response) {
                        // After updating, add the first variant with the new quantity
                  
                   setTimeout(function(){
                        $.ajax({
                            type: 'POST',
                            url: '/cart/add.js',
                            data: {
                                id:  vari_ids[0] ,  // Variant ID
                                quantity: qty,
                                properties: {
                                            product_id: productId  // Send the product_id as a property
                                        }// Quantity to add
                            },
                            dataType: 'json',
                            success: function() {
                                 $.ajax({
                                        url: '/?section_id=cart-drawer',
                                        type: 'GET',
                                        dataType: 'html',
                                        success: function(carthtml) {
                                          $.ajax({
                                            	type: 'GET',
                                            	url: '/cart.json',
                                            	dataType: 'jsonp',
                                            	success: function(data) {
                                                    if(data.item_count == 1){
                                                      var mtk='<div class="cart-count-bubble"><span aria-hidden="true">'+data.item_count+'</span><span class="visually-hidden">'+data.item_count+' item</span></div>';
                                                      $('a#cart-icon-bubble').append(mtk);
                                                    }else{
                                            		$(".cart-count-bubble span:nth-child(1)").text(data.item_count);
                                                    }
                                            	}
                                            });
                                          $("cart-drawer.drawer").addClass("animate");
                                          $("body.gradient").addClass("overflow-hidden");
                                          $(".drawer").addClass("active");
                                          if ($("cart-drawer.drawer").hasClass('is-empty')) {
                                            $('cart-drawer').html($(carthtml).find('cart-drawer').html());
                                            $("cart-drawer.is-empty .drawer__header").css("display", "block");
                                            $("cart-drawer.is-empty .drawer__footer").css("display", "block");
                                            $("cart-drawer.is-empty .cart__contents").css("display", "block");
                                            $("cart-drawer.drawer").removeClass('is-empty');
                                            $("#CartDrawer-Overlay").click(function() {
                                              $("cart-drawer.drawer").removeClass('active');
                                            });
                                          } else {
                                            $("cart-drawer").html($(carthtml).find('cart-drawer').html());
                                          }
                                        }
                                      });
                            },
                            error: function(error) {
                                console.error('Error adding to cart:', error);
                            }
                        });
                     },1000);
                    },
                    error: function(error) {
                        console.error('Error updating cart:', error);
                    }
                });
               },1000);
            } else if (qty == 2) {
              // alert('if2')
              setTimeout(function(){
               
                 $.ajax({
                    type: "POST",
                     url: "/cart/change.js",
                    data: {
                        id: variant_id,
                        quantity: 0  // Set the quantity to 0 to remove the item
                    },
                    success: function(response) {
                   
                        // After updating, add the second variant with the new quantity
                      setTimeout(function(){
                        
                        $.ajax({
                            type: 'POST',
                            url: '/cart/add.js',
                            data: {
                                id:  vari_ids[1] ,  // Variant ID
                                quantity: qty,
                                properties: {
                                            product_id: productId  // Send the product_id as a property
                                        }// Quantity to add
                            },
                            dataType: 'json',
                            success: function() {
                                 $.ajax({
        url: '/?section_id=cart-drawer',
        type: 'GET',
        dataType: 'html',
        success: function(carthtml) {
          $.ajax({
            	type: 'GET',
            	url: '/cart.json',
            	dataType: 'jsonp',
            	success: function(data) {
                    if(data.item_count == 1){
                      var mtk='<div class="cart-count-bubble"><span aria-hidden="true">'+data.item_count+'</span><span class="visually-hidden">'+data.item_count+' item</span></div>';
                      $('a#cart-icon-bubble').append(mtk);
                    }else{
            		$(".cart-count-bubble span:nth-child(1)").text(data.item_count);
                    }
            	}
            });
          $("cart-drawer.drawer").addClass("animate");
          $("body.gradient").addClass("overflow-hidden");
          $(".drawer").addClass("active");
          if ($("cart-drawer.drawer").hasClass('is-empty')) {
            $('cart-drawer').html($(carthtml).find('cart-drawer').html());
            $("cart-drawer.is-empty .drawer__header").css("display", "block");
            $("cart-drawer.is-empty .drawer__footer").css("display", "block");
            $("cart-drawer.is-empty .cart__contents").css("display", "block");
            $("cart-drawer.drawer").removeClass('is-empty');
            $("#CartDrawer-Overlay").click(function() {
              $("cart-drawer.drawer").removeClass('active');
            });
          } else {
            $("cart-drawer").html($(carthtml).find('cart-drawer').html());
          }
        }
      });  // Redirect to the cart page
                            },
                            error: function(error) {
                                console.error('Error adding to cart:', error);
                            }
                        });
                        },1000);
                    },
                    error: function(error) {
                        console.error('Error updating cart:', error);
                    }
                });
              
              }, 1000);
               
            } else {
              // alert('else');
                // Update the third variant's quantity to 0 (removing it)
               setTimeout(function(){
                $.ajax({
                    type: "POST",
                     url: "/cart/change.js",
                    data: {
                        id: variant_id,
                        quantity: 0  // Set the quantity to 0 to remove the item
                    },
                    success: function(response) {
                        // After updating, add the third variant with the new quantity
                      // alert(vari_ids[2]);
                    
                      setTimeout(function(){
                        $.ajax({
                            type: 'POST',
                            url: '/cart/add.js',
                            data: {
                                id: vari_ids[2],  // Variant ID
                                quantity: qty,
                                properties: {
                                            product_id: productId  // Send the product_id as a property
                                        }// Quantity to add
                            },
                            dataType: 'json',
                            success: function() {
                                $('.loader').hide();  // Hide loader if applicable
                                  $.ajax({
        url: '/?section_id=cart-drawer',
        type: 'GET',
        dataType: 'html',
        success: function(carthtml) {
          $.ajax({
            	type: 'GET',
            	url: '/cart.json',
            	dataType: 'jsonp',
            	success: function(data) {
                    if(data.item_count == 1){
                      var mtk='<div class="cart-count-bubble"><span aria-hidden="true">'+data.item_count+'</span><span class="visually-hidden">'+data.item_count+' item</span></div>';
                      $('a#cart-icon-bubble').append(mtk);
                    }else{
            		$(".cart-count-bubble span:nth-child(1)").text(data.item_count);
                    }
            	}
            });
          $("cart-drawer.drawer").addClass("animate");
          $("body.gradient").addClass("overflow-hidden");
          $(".drawer").addClass("active");
          if ($("cart-drawer.drawer").hasClass('is-empty')) {
            $('cart-drawer').html($(carthtml).find('cart-drawer').html());
            $("cart-drawer.is-empty .drawer__header").css("display", "block");
            $("cart-drawer.is-empty .drawer__footer").css("display", "block");
            $("cart-drawer.is-empty .cart__contents").css("display", "block");
            $("cart-drawer.drawer").removeClass('is-empty');
            $("#CartDrawer-Overlay").click(function() {
              $("cart-drawer.drawer").removeClass('active');
            });
          } else {
            $("cart-drawer").html($(carthtml).find('cart-drawer').html());
          }
        }
      });
                            },
                            error: function(error) {
                                console.error('Error adding to cart:', error);
                            }
                        });
                        },1000);
                    },
                    error: function(error) {
                        console.error('Error updating cart:', error);
                    }
                });
                 },2000);
            }
        } else {
            // alert('Product not matched');
        }
    {% endfor %}
});
