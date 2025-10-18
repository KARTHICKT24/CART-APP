# CART-APP

# PROGRAM:
```
import React, { useReducer, useMemo, useCallback, useRef } from "react";

const cartReducer = (state, action) => {
  switch (action.type) {
    case "ADD_ITEM":
      const existing = state.find((item) => item.id === action.payload.id);
      if (existing) {
        return state.map((item) =>
          item.id === action.payload.id
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }
      return [...state, { ...action.payload, quantity: 1 }];

    case "REMOVE_ITEM":
      return state
        .map((item) =>
          item.id === action.payload
            ? { ...item, quantity: item.quantity - 1 }
            : item
        )
        .filter((item) => item.quantity > 0);

    case "CLEAR_CART":
      return [];

    default:
      return state;
  }
};

const Product = ({ product, addToCart, removeFromCart }) => {
  const cardStyle = {
    border: "1px solid #ccc",
    padding: "15px",
    width: "150px",
    textAlign: "center",
    borderRadius: "8px",
    backgroundColor: "#f9f9f9",
  };

  const buttonStyle = {
    margin: "5px",
    padding: "8px 12px",
    border: "none",
    cursor: "pointer",
    borderRadius: "5px",
    color: "white",
  };

  return (
    <div style={cardStyle}>
      <h3>{product.name}</h3>
      <p>₹{product.price}</p>
      <button
        style={{ ...buttonStyle, backgroundColor: "#4caf50" }}
        onClick={() => addToCart(product)}
      >
        Add
      </button>
      <button
        style={{ ...buttonStyle, backgroundColor: "#f44336" }}
        onClick={() => removeFromCart(product.id)}
      >
        Remove
      </button>
    </div>
  );
};

const App = () => {
  const [cart, dispatch] = useReducer(cartReducer, []);

  const products = [
    { id: 1, name: "Shirts", price: 650 },
    { id: 2, name: "Pants", price: 950 },
    { id: 3, name: "Shoes", price: 1200 },
  ];

  const addToCart = useCallback(
    (product) => dispatch({ type: "ADD_ITEM", payload: product }),
    []
  );

  const removeFromCart = useCallback(
    (id) => dispatch({ type: "REMOVE_ITEM", payload: id }),
    []
  );

  const { totalPrice, discount, finalPrice } = useMemo(() => {
    const total = cart.reduce(
      (sum, item) => sum + item.price * item.quantity,
      0
    );
    const discount = total > 2000 ? total * 0.1 : 0;
    const final = total - discount;
    return { totalPrice: total, discount, finalPrice: final };
  }, [cart]);

  const checkoutCount = useRef(0);

  const handleCheckout = () => {
    checkoutCount.current += 1;
    alert(Checkout clicked ${checkoutCount.current} times!);
    dispatch({ type: "CLEAR_CART" });
  };

  const layoutStyle = {
    fontFamily: "Arial, sans-serif",
    padding: "20px",
  };

  const productListStyle = {
    display: "flex",
    gap: "20px",
    marginBottom: "30px",
  };

  const summaryStyle = {
    borderTop: "1px solid #ccc",
    paddingTop: "20px",
  };

  const checkoutBtnStyle = {
    backgroundColor: "#2196f3",
    color: "white",
    padding: "10px 20px",
    border: "none",
    borderRadius: "5px",
    marginTop: "10px",
    cursor: "pointer",
  };

  return (
    <div style={layoutStyle}>
      <h1>Shopping Cart App</h1>

      {/* Product List */}
      <div style={productListStyle}>
        {products.map((product) => (
          <Product
            key={product.id}
            product={product}
            addToCart={addToCart}
            removeFromCart={removeFromCart}
          />
        ))}
      </div>

      {/* Cart Summary */}
      <div style={summaryStyle}>
        <h2>Cart Summary</h2>
        {cart.length === 0 ? (
          <p>Your cart is empty.</p>
        ) : (
          <>
            <ul>
              {cart.map((item) => (
                <li key={item.id}>
                  {item.name} × {item.quantity} = ₹
                  {item.price * item.quantity}
                </li>
              ))}
            </ul>
            <p>Total: ₹{totalPrice}</p>
            <p>Discount: ₹{discount}</p>
            <p>
              <strong>Final Price: ₹{finalPrice}</strong>
            </p>
            <button style={checkoutBtnStyle} onClick={handleCheckout}>
              Checkout
            </button>
          </>
        )}
      </div>
    </div>
  );
};

export default App;
```
# OUTPUT :
![WhatsApp Image 2025-10-18 at 12 07 42_76ba6eb8](https://github.com/user-attachments/assets/1657b33b-0f94-45c6-9164-f564b7b58b7e)
