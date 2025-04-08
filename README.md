# War of Bets Martingale Simulation App
import streamlit as st
import random
import pandas as pd
import plotly.express as px

# App Title
st.title('War of Bets - Martingale Strategy Simulator')

# User Inputs
starting_balance = st.number_input('Starting Balance', min_value=100, value=1000, step=100)
base_bet = st.number_input('Base Bet Amount', min_value=1, value=10, step=1)
rounds = st.number_input('Number of Rounds', min_value=1, value=50, step=1)

# Martingale Simulation Function
def martingale_simulation(starting_balance, base_bet, rounds):
    balance = starting_balance
    current_bet = base_bet
    results = []

    for round_num in range(1, rounds + 1):
        win = random.choice([True, False])
        if win:
            balance += current_bet
            current_bet = base_bet
        else:
            balance -= current_bet
            current_bet = min(balance, current_bet * 2) if balance >= current_bet * 2 else balance

        results.append({'Round': round_num, 'Balance': balance})

        if balance <= 0:
            break

    return pd.DataFrame(results)

# Run Simulation Button
if st.button('Run Simulation'):
    df = martingale_simulation(starting_balance, base_bet, rounds)

    # Show Data Table
    st.write(f"Final Balance after {len(df)} rounds: {df['Balance'].iloc[-1]}")
    st.dataframe(df)

    # Show Balance Over Time Chart
    fig = px.line(df, x='Round', y='Balance', title='Balance Over Time')
    st.plotly_chart(fig)
# War-of-bets-simulator-
